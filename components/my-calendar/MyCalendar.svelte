

<div class="field">
  <label class="label">
    {#if label}
    <div class="text">{label}</div>
    {/if}
    <div class="control is-medium">
      <Calendar bind:selected="value" bind:dateChosen="dateChosen" bind:format bind:formattedSelected="formattedSelected">
        <button class="button is-outline">
          {#if dateChosen} {formattedSelected} {:else} Choose expiry {/if}
        </button>
      </Calendar>
    </div>
  </label>
</div>

<style>
  @import '//cdnjs.cloudflare.com/ajax/libs/bulma/0.7.4/css/bulma.css';

  label {
    font-size: 18px;
    color: #000;
  }

  .control {
    margin-top: 12px;
  }
</style>

<script>
  export default {
    data () {
      return {
        label: undefined,
        value: new Date(),
        dateChosen: false,
        format: '#{D}, #{M} #{j}, #{Y}',
        validators: [],
        dirty: false,
        validationMessage: undefined,
        formattedSelected: undefined
      }
    },

    oncreate () {
      this.validate()
    },

    onupdate({ changed, current, previous }) {
      if (changed.dateChosen && current.dateChosen === true) {
        this.validate(true)
      }
    },
    
    methods: {
      validate (dirty) {
        const { validators, value, dateChosen } = this.get()
        if (!dateChosen) {
          return this.set({ dirty: false, valid: false, validationMessage: undefined })
        }
        const failing = validators.find(v => v(value) !== true)
        const valid = !failing
        const validationMessage = failing && failing(value)
        this.set({ dirty, valid, validationMessage })
      }
    },

    components: {
      Calendar: 'svelte-calendar'
    }
  }
</script>