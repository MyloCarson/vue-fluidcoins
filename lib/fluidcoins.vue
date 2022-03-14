<template>
    <button @click="payWithFluidcoins" class="fluidcoins-button" :class="[buttonCSSClass, {'fluidcoins-button--rounded': rounded}]" aria-label="pay with fluidcoins">
        <div class="fluidcoins-logo__wrapper">
            <img src="./assets/fluidcoins.svg" alt="fluidcoins logo" class="fluidcoins-logo" />
        </div>
        <span class="fluidcoins-button__label" :class="[buttonLabelCSSClass]">
            Pay with Fluidcoins
        </span>
    </button>
</template>

<script setup>
import { onBeforeMount, ref } from "vue";

const props = defineProps({
    type: {
        type: String,
        default: 'primary',
        validator: (value) => {
            return ['secondary', 'primary'].includes(value)
        }
    },
    rounded: {
        type: Boolean,
        default: true
    },
    publicKey: {
        type: String,
        required: true
    },
    amount: {
        type: Number,
        required: true,
        validator: (value)=> {
            return value > 0
        },
    },
    email: {
        type: String,
        required: true
    },
    onSuccess: {
        type: Function,
        required: true
    },
    name: String,
    phone: String,
    callback_url: String,
    currency: String,
    onLoad: {
        type: Function,
        default: () => {}
    },
    onClose: {
        type: Function,
        default: () => {}
    },
    onError: {
        type: Function,
        default: () => {}
    },
    metadata: {
        type: Object,
        default: () => {}
    }
})
const scriptLoaded = ref(null);

const isPrimary = props.type === 'primary'
const buttonCSSClass = isPrimary ? 'fluidcoins-button--primary' :  'fluidcoins-button--secondary'
const buttonLabelCSSClass = isPrimary ? 'fluidcoins-button__label--primary' : 'fluidcoins-button__label--secondary'

onBeforeMount(() => {
    loadScript()
    .then(() => {
        scriptLoaded.value = true
    }).catch(() => {
        scriptLoaded.value = false
    })
});

const payWithFluidcoins = () => {
    let config = { key: props.publicKey}
    Object.entries(props).forEach(([k, v]) => {
        if(!['type', 'rounded', 'publicKey'].includes(k) && v){
            config[k] = v
        }
    })

    if(!scriptLoaded.value) {
        return;
    }
    
    const fPay = new window.Fluidcoins(config)
    fPay.setup()
    fPay.open()

}






function loadScript() {
  // https://github.com/tserkov/vue-plugin-load-script/blob/vue3/index.js
  const SCRIPT_SRC = 'https://js.fluidcoins.com/pay.js'
  return new Promise(function (resolve, reject) {
    let shouldAppend = false;
    let el = document.querySelector('script[src="' + SCRIPT_SRC + '"]');
    if (!el) {
      el = document.createElement("script");
      el.type = "text/javascript";
      el.async = false;
      el.src = SCRIPT_SRC;
      shouldAppend = true;
    } else if (el.hasAttribute("data-loaded")) {
      resolve(el);
      return;
    }

    el.addEventListener("error", reject);
    el.addEventListener("abort", reject);
    el.addEventListener("load", function loadScriptHandler() {
      el.setAttribute("data-loaded", true);
      resolve(el);
    });

    if (shouldAppend) document.head.appendChild(el);
  });
}
</script>

<style lang="css" scoped>

@import url(https://res.cloudinary.com/fluidcoins/raw/upload/v1637590555/Typography/ClashDisplay/ClashDisplay-Variable-FontFace_aw31h4.css);
.fluidcoins-button {
    --navy-blue: #032343;
    --gradient-1: #A8FF78;
    --gradient-2: #1FFFBB;

    display: flex;
    align-items: center;
    padding: 0.625rem 1.5rem;
    border: 0;
    cursor: pointer;
}
.fluidcoins-button--rounded{
    border-radius: 2rem;
}
.fluidcoins-button--secondary {
    background: linear-gradient(305.17deg, var(--gradient-1) 15.89%, var(--gradient-2) 237.61%);
}
.fluidcoins-button--primary {
    background: var(--navy-blue);
}
.fluidcoins-button__label {
    font-family: 'ClashDisplay', Helvetica, Arial, sans-serif;
    font-size: 1.25rem;
    font-weight: 500;
    line-height: 120%;
    
    margin-left: 1.5rem;
}

.fluidcoins-button__label--secondary{
    color: var(--navy-blue);
}
.fluidcoins-button__label--primary {
    background-image: linear-gradient(
      338.85deg,
      var(--gradient-1) -9.64%,
      var(--gradient-2) 206.38%
    );
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }
.fluidcoins-logo {
    height: 100%;
    max-width: 100%;
}
.fluidcoins-logo__wrapper {
    height: 2.5rem;
    width: 2.5rem;
}
</style>