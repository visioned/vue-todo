<template>
  <div class="about">
    <h1>About</h1>
    <p>
      <strong>Vue Todos</strong> is a CRUD Todo tracking application created in
      Vue 3. This was created as a learning exercise to learn Vue 3 and the new
      composition API.
      <br />
      <br />
      The application is simple, it allows you to create, read, update, and
      delete todos. Using local storage, we can save the todos within the
      browser, but this can easily be changed to use a database for real world
      persistence.
    </p>
  </div>
</template>

<style lang="scss">
.about {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;
  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
}
</style>
