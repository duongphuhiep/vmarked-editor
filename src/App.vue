<template>
  <div id="app">

    <VMarkedEditor class='ed' v-model="c2">
# Title 1

Lorem ipsum *dolor* sit amet **consectetur adipisicing elit**. Quibusdam `nulla`, tenetur maxime aliquid quas expedita distinctio sunt fugit quam enim, ducimus voluptatem voluptas quos libero nobis eos asperiores maiores laboriosam.

## Title 1b

```php
&lt;?php
function nfact($n) {
    if ($n == 0) {
        return 1;
    }
    else {
        return $n * nfact($n - 1);
    }
}

echo "\n\nPlease enter a whole number ... ";
$num = trim(fgets(STDIN));

// ===== PROCESS - Determing the factorial of the input number =====
$output = "\n\nFactorial " . $num . " = " . nfact($num) . "\n\n";
echo $output;
```
  </VMarkedEditor>
  </div>
</template>

<script>
import VMarkedEditor from "./components/VMarkedEditor.vue";

export default {
  name: "app",
  data() {
    return {
      c1: "",
      c2: ""
    };
  },
  components: {
    VMarkedEditor
  }
};
</script>

<style>
.ed {
  height: 500px;
  width: 100%;
}
</style>
