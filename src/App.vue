<script setup>
  import { ref, reactive, onMounted, watch } from 'vue'
  import { uid } from 'uid'

  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Paciente from './components/Paciente.vue';

  const patients = ref([])

  const patient = reactive({
      id: null,
      name: '',
      owner: '',
      email: '',
      dischargeDate: '',
      symptoms: ''
  })

  onMounted(() => {
    const storePatiets = localStorage.getItem('patients')
    if(storePatiets) {
        patients.value = JSON.parse(storePatiets)
    }
  })

  const savePatient = () => {
    if(patient.id) {
      const { id } = patient
      const i = patients.value.findIndex( patient => patient.id === id)
      patients.value[i] = {...patient}
    } else {
      patients.value.push({
        ...patient, //es una copia de patient por tanto no reactivo
        id: uid()
      })
    }

    // Reset object (option 1)
    /*patient.name = ''
    patient.owner = ''
    patient.email = ''
    patient.dischargeDate = ''
    patient.symptoms = ''*/

    // Reset object (option 2)
    Object.assign(patient, {
      id: null,
      name: '',
      owner: '',
      email: '',
      dischargeDate: '',
      symptoms: ''
    })
  }

  const updatePatient = id => {
      const patientEdit = patients.value.filter( patient => patient.id === id)[0]
      Object.assign(patient, patientEdit)
    }

  const deletePatient = id => {
      patients.value = patients.value.filter( patient => patient.id !== id)
  }

  const saveInLocalStorage = () => {
    localStorage.setItem('patients', JSON.stringify(patients.value))
  }

  watch(patients, () => saveInLocalStorage(), {
    deep: true
  })
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario
        v-model:name="patient.name"
        v-model:owner="patient.owner"
        v-model:email="patient.email"
        v-model:dischargeDate="patient.dischargeDate"
        v-model:symptoms="patient.symptoms"
        :id="patient.id"
        @save-patient="savePatient"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">
          Administrar tus pacientes
        </h3>
        <div v-if="patients.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
              Información de
              <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Paciente
            v-for="patient in patients"
            :patient="patient"
            @update-patient="updatePatient"
            @delete-patient="deletePatient"
          />
          </div>
          <p v-else class="mt-20 text-2xl text-center">
            No hay pacientes
          </p>
      </div>
    </div>
  </div>
</template>