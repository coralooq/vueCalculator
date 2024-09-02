<script setup>
    import { ref } from 'vue';

    let result = ref('');
    let sign = ref(false);
    let allowMinus = ref(false);
    let value;
    let regexp = /(\-?\d*?\.?\d+)([\+\-\*\/])(\-?\d+)/;

    function calculating(event) {    
        if(event.target.tagName != 'BUTTON') return;
        result.value += event.target.innerHTML;  
        if(event.target.innerHTML == '=') {
            result.value = value;
            sign.value = false;
            allowMinus.value = false;
        };
        if(event.target.innerHTML == 'c') {
            result.value = '';
            sign.value = false;
            allowMinus.value = false;
        } 
        if(result.value[0] == 'c') result.value = '';
        if(result.value[result.value.length - 1] == '=') result.value = result.value.slice(0,-1);
       
        let check = result.value.match(regexp);
       
        if(check) {
           let firstNumber = +check[1];
           let sign = check[2];
           let secondNumber = +check[3];
           if(sign == '+') value = firstNumber + secondNumber;
           if(sign == '-') value = firstNumber - secondNumber;
           if(sign == '*') value = firstNumber * secondNumber;
           if(sign == '/') value = firstNumber / secondNumber;
           if(!Number.isInteger(value)) value = value.toFixed(5); 
        };
        
        if(result.value[result.value.length - 1].match(/[\+\-\*\/]/) && check) {
            console.log(value);
            result.value = value + event.target.innerHTML;
        };
        if(result.value[result.value.length - 1].match(/[\+\-\*\/]/) || check[3].match(/\-/)) {
            allowMinus.value = true
            sign.value = true;
        } else sign.value = false
        if(result.value[result.value.length - 1].match(/[\*\/]/) || check) allowMinus.value = false;
        if(result.value[result.value.length - 1].match(/[\+\-]/) || check[3].match(/\-/)) allowMinus.value = true;
    }
</script>

<template>
    <div id="container">
    <table @click='calculating' id="table">
        <tr><td colspan="4"><input id="result" type="text" v-model='result' disabled ></td></tr>
        <tr><td><button>7</button></td><td><button>8</button></td><td><button>9</button></td><td><button :disabled='sign'>*</button></td></tr>
        <tr><td><button>4</button></td><td><button>5</button></td><td><button>6</button></td><td><button :disabled='allowMinus'>-</button></td></tr>
        <tr><td><button>1</button></td><td><button>2</button></td><td><button>3</button></td><td><button :disabled='sign'>+</button></td></tr>
        <tr><td><button>c</button></td><td><button>0</button></td><td><button>=</button></td><td><button :disabled='sign'>/</button></td></tr>
    </table>
    </div>
</template>

<style>
    #result {
        width: 256px;
        height: 30px;
        font-size: 28px;
    }

    #container {
        display: flex;
        justify-content: center;
        margin-top: 200px;
    }

    table {
        border-collapse: collapse;
    }

    button {
        border: 1px rgb(240, 237, 237) solid;
        margin: 1px;
    }

    button:hover {
        background-color: antiquewhite;
        cursor: pointer;
    }

    button {
        width: 60px;
        height: 60px;
        font-size: 30px;
    }


</style>