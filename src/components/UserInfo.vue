<template>
  <div class="container">
    <div class="card card-login mx-auto text-center bg-light">
      <div class="card-header mx-auto bg-light">
        <span> <img alt="Vue logo" src="../assets/logo-eev-sas.png"> </span><br/>
      </div>
      <div class="card-body">
        <form action="" method="post">
          <div class="input-group form-group">
            <div class="input-group-prepend">
              <span class="input-group-text"><i class="fas fa-user"></i></span>
            </div>
            <input type="text" name="nom" class="form-control" placeholder="Nom ...">
          </div>

          <div class="input-group form-group">
            <div class="input-group-prepend">
              <span class="input-group-text"><i class="fas fa-user"></i></span>
            </div>
            <input type="text" name="prenom" class="form-control" placeholder="Prénom ...">
          </div>

          <div class="input-group form-group">
            <div class="input-group-prepend">
              <span class="input-group-text"><i class="fas fa-industry"></i></span>
            </div>
            <input type="text" name="societe" class="form-control" placeholder="Société ...">
          </div>

          <div class="form-group">
            <button type="submit" name="btn" class="btn btn-outline-danger float-right login_btn">Commencer</button>
          </div>

        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
