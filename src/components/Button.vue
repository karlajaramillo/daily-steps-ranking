<template lang="">
    <button class="button">{{text}}</button>
</template>
<script>
export default {
  name: "Button",
  props: {
    text: String,
  },
};
</script>

<style scoped>
.button {
  border: 1px solid #ff5f4f;
  color: white;
  cursor: pointer;
  transition: all 0.2s linear;
  margin-top: 30px;
  font-size: 1rem;
  padding: 10px 20px;
  background-color: #ff5f4f;
  border-radius: 4px;
  font-weight: 500;
  text-transform: uppercase;
  font-family: "Sora";
}

.button:focus {
  outline: none;
}

.button:hover,
.button:active {
  background: #ce4437;
  border-color: #ce4437;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.26);
}

@media (min-width: 768px) {
  .button {
    font-size: 2rem;
  }
}
</style>