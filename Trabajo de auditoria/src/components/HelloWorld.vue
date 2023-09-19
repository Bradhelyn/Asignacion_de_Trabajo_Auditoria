<template>
  <div class="container">
    <div>


</div>
    <article>
      <header>
        <div class="progress">
          <div class="progress-step"
          :class="{'active':index === activeStep}"
          v-for="(step, index) in formSteps"
          :key="'step'+index">
            {{ index + 1 }}
          </div>
        </div>
      </header>
      <section :class="animation">
        <h1 class="title">Control de Trabajo</h1>


<div class="card is-large">
  </div>
        <h2>{{ formSteps[activeStep].title }}</h2>
        <div class="input-fields">
          <div class="input-container"
          v-for="(field, index) in formSteps[activeStep].fields"
          :key="'field'+index">
            <input :type="field.type" type="text" :class="{'wrong-input': !field.valid}" v-model="field.value" required>
            <label class="input-label">{{ field.label }}</label>
            
          </div>
        </div>
        <div class="actions">
          <button @click="previousStep" v-if="activeStep > 0" >Atrás</button>
          <button v-if="activeStep +1 < formSteps.length -1" @click="checkFields">Siguiente</button>
          <button v-if="activeStep +1 === formSteps.length -1" @click="checkFields">Guardar</button>
        </div>
      </section>
    </article>
  </div>
</template>

<script>
  export default {
  data: () => {
    return {
      activeStep: 0,
      animation: 'animate-in',
      formSteps: [
        {
          title: "1.Información General",
          fields: [
            { type:"text", label: "Titulo Auditoría", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Num. Asignación", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Área Auditada", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Tipo de Auditoría", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Dirección/Gerencia Auditoría", value: '', valid: true, pattern: /.+/ },
            { type:"date", label: "Fecha Asignación", value: "", valid: true, pattern: /.+/ },
            { type:"text", label: "Auditor(a) a Cargo", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Origen Auditoría", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Gerente a Cargo", value: '', valid: true, pattern: /.+/ }

          ]
        },
        {
          title: "2.Planificación",
          fields: [
            { type:"date", label: "Inicio", value: '', valid: true, pattern: /.+/ },
            { type:"date", label: "Fin", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Total Horas Presupuestadas", value: '', valid: true, pattern: /.+/ }
          ]
        },
        {
          title: "3.Recursos",
          fields: [
            { type:"text", label: "Personal Asignado", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Cargo", value: '', valid: true, pattern: /.+/ },
            { type:"text", label: "Horas/Hombre", value: '', valid: true, pattern: /.+/ }
          ]
        },
        {
          title: "Guardado exitosamente",
        }
      ],
    }
  },
    methods: {
      nextStep() {
        this.animation = 'animate-out';
        setTimeout(() => {
          this.animation = 'animate-in';
          this.activeStep += 1;
        }, 550);
      },
      checkFields() {
        let valid = true;
        this.formSteps[this.activeStep].fields.forEach(field => {
          if(!field.pattern.test(field.value)) {
            valid = false;
            field.valid = false;
          }
          else {
            field.valid = true;
          }
        });

        if(valid) {
          this.nextStep();
        }
        else {
          this.animation = 'animate-wrong';
          setTimeout(() => {
            this.animation = '';
          }, 400);
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css?family=Noto+Sans&display=swap');

  @mixin flexbox() {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .container {
    @include flexbox();
    width: 100%;
    min-height: 100vh;
    font-family: 'Noto Sans', sans-serif;
    background: radial-gradient(#063970, rgba(6, 57, 112));
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 8000px;
    max-width: 900px;
    width: 1050px;
    border-radius: 6px;
    padding: 30px;
    margin: 0 15px;
  }

  article {
    display: flex;
    margin: 10px;
    width: calc(100% - 20px);
    max-width: 720px;
    min-height: 480px;
    perspective: 1000px;

    header {
      @include flexbox();
      width: 60px;
      height: 480px;
      background-color: #fff;
      border-right: 2px dotted #003871;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);
    }

    .progress-step {
      @include flexbox();
      position: relative;
      width: 30px;
      height: 20px;
      border-radius: 50%;
      margin-bottom: 20px;
      color: #fff;
      background-color: #DF5C2E;
      font-weight: bold;

      &.active {
        background-color: #003871;

        ~ .progress-step {
          color: #003871;
          background-color: #ccc;
        }

        ~ .progress-step::before {
          background-color: #ccc;
        }
      }

      &:before {
        content: '';
        position: absolute;
        top: -20px;
        width: 2px;
        height: 20px;
        background-color: #DF5C2E;
        z-index: 10;
      }

      &:first-child::before {
        display: none;
      }
    }

    section {
      @include flexbox();
      flex-direction: column;
      width: 100%;
      background-color: #fff;
      box-shadow: 0 15px 30px rgba(0,0,0,.2),
                  0 10px 10px rgba(0,0,0,.2);

      h2 {
        font-size: 1.6rem;
        color: black;
        margin: 0;
        padding: 20px;
      }

      .input-fields {
        @include flexbox();
        display: grid;
        grid-template-columns: repeat(2,1fr);
        grid-gap: .75rem 1rem;
        width: 100%;
      }

      .input-container {
        position: relative;
        padding: 30px 20px 20px 20px;
        width: calc(100% - 40px);
        max-width: 400px;

        input {
          position: relative;
          width: 100%;
          font-family: 'Noto Sans', sans-serif;
          font-size: 1.35rem;
          outline: none;
          background: transparent;
          box-shadow: none;
          border: none;
          border-bottom: 2px dashed #003871;

          &:valid + .input-label {
            top: 10px;
            left: 20px;
            font-size: .7rem;
            font-weight: normal;
            color: #999;
          }

          &.wrong-input + .input-label {
            color: #DF5C2E;
          }
        }
      }

      .input-label {
        position: absolute;
        top: 32px;
        left: 20px;
      
        pointer-events: none;
        transition: .2s ease-in-out;
      }

      .actions {
        margin: 0;

        button {
          font-family: 'Noto Sans', sans-serif;
          outline: none;
          border: none;
          color: #fff;
          background-color: #DF5C2E;
          font-size: 1.35rem;
          padding: 5px 20px;
          margin: 0;
          text-transform: uppercase;
          border-radius: 3px;
          cursor: pointer;
        }
      }
    }
  }

  .animate-in {
    transform-origin: left;
    animation: in .6s ease-in-out;
  }

  .animate-out {
    transform-origin: bottom left;
    animation: out .6s ease-in-out;
  }

  .animate-wrong {
    animation: wrong .4s ease-in-out;
  }

  @keyframes in {
    0% {
      opacity: 0;
      transform: rotateY(90deg);
    }
    100% {
      opacity: 1;
      transform: rotateY(0deg);
    }
  }

  @keyframes out {
    0% { transform: translateY(0px) rotate(0deg); }
    60% { transform: rotate(10deg); }
    100% { transform: translateY(1000px); }
  }

  @keyframes wrong {
    0% { transform: translateX(0); }
    20% { transform: translateX(40px); }
    40% { transform: translateX(20px); }
    60% { transform: translateX(40px); }
    80% { transform: translateX(20px); }
    100% { transform: translateX(0); }
  }
</style>
