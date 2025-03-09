---
{"dg-publish":true,"permalink":"/02-pages/Vue 父子组件/","tags":["personal/blog","program/frontend/vue3"]}
---

当前是子组件 `MdEditor.vue`，在其他地方（例如父组件），我们调用该组件的方式是 `<MdEditor />`。我们想要在能够在上一级组件中定义这个组件的一些形式，例如指定该组件的一些值，比如说：
```vue
<MdEditor :value="value" :handle-change="onChange"></MdEditor>
```
那么，我们就需要在 `MdEditor.vue` 中定义一些暴露的变量，若要暴露上述变量，我们需要在 `MdEditor.md` 中定义如下：
```vue
<template>  
  ...
</template>  
  
<script setup lang="ts">  
...
  
/**  
 * @description Props interface for the MdEditor component * Defines the component's properties that can be passed from parent components 
 */
 interface Props {  
  /**  
   * The markdown content to be edited   
   */  
   value: string;  
  
  /**  
   * Callback function that is triggered when the markdown content changes   * @param {string} value - The new markdown content  
   */  
   handleChange: (value: string) => void;  
}  
  
/**  
 * Component props with default values * Initializes the component with empty content and a basic change handler */
 const props = withDefaults(defineProps<Props>(), {  
  value: "",  
  handleChange: (v: string) => {  
    console.log(v);  
  },  
});  
</script>  
  
<style scoped></style>
```
