TOS sur l'implémentation des layouts dans une application NUXT3.

## Pourquoi créer un layout

Forcément, si vous avez plusieurs parties de votre application qui aurons des `navbar/sidebar` différentes vous serez obligé d'utiliser des layouts. Cependant, même si votre application n'a pas besoin de layouts en sois, vous allez pouvoir créer un layout que nous allons appeler `default`, en créant ce layout vous allez pouvoir supprimer le fichier `app.vue` à la racine ce qui vous permet de supprimer cette couche pour la transférer vers le layout. Cette pratique est recommandée par beaucoup de développeurs, car il est rare d'avoir un site avec un seul layout, ainsi peu importe le projet où vous êtes, vous savez que tout se passe dans les layouts !

## Créer nos layout
La première chose à faire va être de créer un dossier `layouts`, dans celui-ci, vous allez pouvoir créer un composant vue appeler `default`. Ce layout sera alors utiliser par défaut sur toutes les pages ou vous ne spécifiez pas de layouts précis. Vous pouvez maintenant supprimer le fichier `app.vue` à la racine qui est maintenant inutile !

Vous pouvez ensuite créer les autres layouts dont vous avez besoin, dans mon cas, je crée un layout `default` et un layout `login`, car ma page de login n'aura pas de `navbar` et de `footer` :

![image](https://github.com/SteeveGllt/tos_nuxt_layouts/assets/71708517/245070e0-9cd4-4779-8b35-027db068f6a2)

Mon `layout` est un composant Vue, il contient donc une partie `template`, `script` et `style` :

`template` : L'élément important dans la `template` est le `<slot/>`, l'attribut `slot` est l'endroit ou le code des html des `pages` sera injecter. Ici j'ajoute donc ma `Navbar` et mon `Footer` qui serons ajouter respectivement avant et après le contenu de mes pages.

`script` : Dans ma balise `script`, j'importe mon `Footer` et ma `Navbar`, ensuite, je déclare mon composant et je lui ajoute ma `Navbar` et mon `Footer` dans ces composants interne. Bien évidemment, je peux effectuer n'importe quelle action comme dans n'importe quel composant Vue.

`style` : J'importe mon framework css `Bulma`, ainsi, il sera importé sur toutes mes pages qui utilisent ce `layout`.

```
<template>
  <div>
    <Navbar></Navbar>
    <slot/>

    <Footer></Footer>
  </div>

</template>

<script lang="ts">
import {defineComponent} from "vue";
import { Footer, Navbar} from "#components";


export default defineComponent({
  components: {Navbar, Footer}
})
</script>

<style>
@import "@/assets/css/bulma.min.css";
</style>
```

Enfin pour mon layout `login`, je veux juste qu'il affiche ma page seule sans élément en plus avec mon framework css `Bulma` :

```
<template>
    <slot/>
</template>

<script lang="ts">
export default defineComponent({})
</script>

<style>
@import "@/assets/css/bulma.min.css";
</style>
```

## Utiliser nos layouts
Une fois nos layouts crée, il faut les utiliser. Pour le layout `default` il n'y a rien à faire puisqu'il sera utilisé par défaut pour toutes les pages qui ne précisent pas de layouts, cependant pour les autres layouts il faudra les spécifier dans les pages en questions

Voici ma page `login.vue` :
- La partie qui nous intéresse est le `definePageMeta()`, en lui donnant en paramètre un objet avec un attribut `layout`, il nous suffit de lui donner le nom du layout et automatiquement cette page utiliseras le layout définit !
```
<script lang="ts">
import { UserLoginReq } from 'interfaces/User';

definePageMeta({
      layout: "login",
    }
);

export default defineComponent({
  name: "login",
  data: () => {
    return {
      userReq: {} as UserLoginReq,
    }
  },
  methods: {
    login() {
      console.log("Login à coder");
    }
  }
})
</script>

<template>
  <div class="hero is-fullheight has_background_gradient">
    <div class="hero-body is-justify-content-center is-align-items-center">
      <div class="columns is-flex is-flex-direction-column box">
        <form @submit.prevent="login()">
          <div class="column">
            <h1 class="title has-text-centered">Login</h1>
          </div>
          <div class="column">
            <label for="email">Username</label>
            <input v-model="userReq.username" class="input is-primary" type="text" name="email" id="email" placeholder="Username">
          </div>
          <div class="column">
            <label for="Name">Password</label>
            <input v-model="userReq.password" class="input is-primary" type="password" name="Name" id="Name" placeholder="Password">
          </div>
          <div class="column">
            <button class="button is-primary is-fullwidth" type="submit">Login</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped>
.has_background_gradient {
  background:linear-gradient(120deg, #00d1b2, #FFFFFF);
}
</style>

```
Ainsi, je peux me rendre sur ma page `login` et en ouvrant le `NuxtDevTools`, je peux voir que la page utilise bel et bien le layout `login` :
![Capture_dcran_du_2023-08-02_22-32-17](https://github.com/SteeveGllt/tos_nuxt_layouts/assets/71708517/1ffc5def-0904-4689-be72-87ab2dddc1df)


Alors que quand je vais sur ma page `index`, je peux voir que ma `Navbar` et mon `Footer` sont bien ajouter et le `NuxtDevTools` m'informe également que cette page utilise le layout `default` :
![Capture_dcran_du_2023-08-02_22-34-05](https://github.com/SteeveGllt/tos_nuxt_layouts/assets/71708517/f6958ebb-cdcf-4206-8a35-801649d8141b)
