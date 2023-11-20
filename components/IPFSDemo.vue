<script setup lang="ts">
import * as odd from '@oddjs/odd'

const appInfo = { creator: "daffl", name: "local-first-demos" }

odd.program({
  // `namespace` can also be just a string; it's used as an identifier for caches.
  // If you're developing multiple apps on the same localhost port,
  // make sure these differ.
  namespace: appInfo

}).catch(error => {
  console.log(error)
  switch (error) {
    case odd.ProgramError.InsecureContext:
      // ODD requires HTTPS
      break;
    case odd.ProgramError.UnsupportedBrowser:
      // Browsers must support IndexedDB
      break;
  }

}).then(async p => {
  const program = p as odd.Program
  const username = "daffl"

  // Check if username is valid and available
  const valid = await program.auth.isUsernameValid(username)
  const available = await program.auth.isUsernameAvailable(username)

  if (valid && available) {
    // Register the user
    const { success } = await program.auth.register({ username })
    
    if (!success) {
      throw new Error('Something went wrong')
    }

    // Create a session on success
    const session = await program.auth.session()
    const fs = session?.fs!

    // Create a sub directory and write to a file
    await fs.write(
      odd.path.appData(appInfo, odd.path.file('public', 'hello.txt')),
      new TextEncoder().encode('Hello everyone! 👋')
    )

    // Persist changes and announce them to your other devices
    await fs.publish()

    // Read from a file
    const content = new TextDecoder().decode(
      await fs.read(
        odd.path.appData(appInfo, odd.path.file("public", "hello.txt"))
      )
    )

    console.log(content)
  }
})

</script>

<template>
  <h1>IPFS Demo</h1>
</template>