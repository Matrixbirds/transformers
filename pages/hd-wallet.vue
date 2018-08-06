<template>
  <div class='container grid-960'>
    <h1>HD Wallet</h1>
    <section class='container'>
      <div class='columns'>
        <div class='column row-6'>
          <div class='form-group'>
            <input class='form-input' :value='mnemonicLength' type='number' placeholder='mnemonic length default is 12' />
          </div>
          <div class='form-group'>
            <button @click='genMnemonic' class='btn btn-sm'>Generate Mnemonic</button>
          </div>
          <div class='form-group'>
            <h6>Mnemonic</h6>
            <textarea class='form-input' rows='3' readonly :value='mnemonic'></textarea>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  components: {
    VlCode: () => import('~/components/vl-code'),
  },
  data() {
    return {
      locale: 'en',
      content: {},
      mnemonicLength: null,
      mnemonic: null,
      active: null,
      error: null,
      mounted: false,
      optionsQR: null,
    };
  },
  watch: {
    'content.result': 'updateQR',
    'content.label': 'updateQR',
  },
  methods: {
    genMnemonic() {
      this.locale = 'en';
      const len = this.mnemonicLength || 12;
      this.mnemonic = genMnemonic(len);
    },
    updateQR() {
      const { result, label } = this.content;
      const logo = label && {
        text: label,
        clearEdges: 3,
      };
      this.optionsQR = {
        cellSize: 4,
        data: result,
        logo,
      };
    },
    onReset() {
      this.active = null;
      this.content = {
        name: null,
        label: null,
        config: '',
        result: null,
      };
    },
    onChange: function onChange(data) {
      if (data === this.cachedData) return;
      this.cachedData = data;
      this.content.config = data;
      this.onUpdate();
    },
    onUpdate() {
    },
    onSave() {
    },
  },
  created() {
    this.onReset();
  },
  mounted() {
    this.mounted = true;
  },
};

function genMnemonic(length) {
  const chars = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'];
  const words = [];
  let mnemonicLength = length;
  while (mnemonicLength) {
    let wordLen = Math.min(3, Math.floor(Math.random() * 12));
    let word = '';
    while (wordLen) {
      const i = Math.floor(Math.random() * 25);
      word += chars[i];
      wordLen -= 1;
    }
    words.push(word);
    mnemonicLength -= 1;
  }
  return words.join(' ');
}
</script>

<style>
.t-code {
  border: 1px solid #caced7;
  > .CodeMirror {
    height: 400px;
  }
}

.t-url {
  word-break: break-all;
}
</style>
