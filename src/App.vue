<template>
  <router-view />
  
</template>

<style>
/* font-family: 'Quicksand', sans-serif;
font-family: 'Sora', sans-serif;
font-family: 'Zen Kaku Gothic Antique', sans-serif; */

* *,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* Remove default padding */
ul[class],
ol[class] {
  padding: 0;
}

/* Remove default margin */
body,
h1,
h2,
h3,
h4,
p,
ul[class],
ol[class],
li,
figure,
figcaption,
blockquote,
dl,
dd {
  margin: 0;
}

/* Set core body defaults */
body {
  min-height: 100vh;
  scroll-behavior: smooth;
  text-rendering: optimizeSpeed;
}

/* Remove list styles on ul, ol elements with a class attribute */
ul[class],
ol[class] {
  list-style: none;
}
/* Make images easier to work with */
img {
  max-width: 100%;
}

/* Natural flow and rhythm in articles by default */
article > * + * {
  margin-top: 1em;
}

/* Inherit fonts for inputs and buttons */
input,
button,
textarea,
select {
  font: inherit;
}

a {
  text-decoration: none;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

body {
  font: 1rem "Zen Kaku Gothic Antique", "Open Sans", Helvetica, Arial,
    sans-serif;
  line-height: 1.5;
  width: 100%;
  height: auto;
  margin: 0;
 background-color: #f5f5f5;
  color: #333333;
}

img {
  width: 100%;
}
@media (min-width: 850px) {
  p {
    font-size: 1.5rem;
  }
  h2 {
    font-size: 1.8rem;
  }
  li {
    font-size: 1.2rem;
  }
  a {
    font-size: 1.2rem;
  }
}
</style>
