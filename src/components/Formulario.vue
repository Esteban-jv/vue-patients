<script setup>
    import { reactive, computed } from 'vue'
    import Alerta from './Alerta.vue';

    const alert = reactive({
        type: '',
        message: ''
    })

    const editing = computed(() => {
        return props.id
    })

    const emits = defineEmits([
        'update:name',
        'update:owner',
        'update:email',
        'update:discharge-date',
        'update:symptoms',

        'savePatient'
    ])

    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        name: {
            type: String,
            required: true
        },
        owner: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        dischargeDate: {
            type: String,
            required: true
        },
        symptoms: {
            type: String,
            required: true
        }
    })

    const validate = () => {
        if(Object.values(props).includes(''))// if([patient.name, patient.owner].includes(''))
        {
            alert.message = 'Todos los campos son obligatorios'
            alert.type = 'error'
            resetAlert()
            return
        }
        
        emits('savePatient')
        alert.message = 'Paciente almacenado correctamente'
        alert.type = 'success'
        resetAlert()
    }

    const resetAlert = (time=3000) => {
        setTimeout(() => {
            Object.assign(alert, {
                type: '',
                message: ''
            })
        }, time)
    }
</script>
<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento de pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>

        <Alerta
            v-if="alert.message"
            :alert="alert"
        />

        <form
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validate"
        >
            <div class="mb-5">
                <label
                    for="pet"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Mascota
                </label>
                <input
                    type="text"
                    id="pet"
                    placeholder="Nombre de la mascota"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="name"
                    @input="$emit('update:name', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label
                    for="owner"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Nombre del propietario
                </label>
                <input
                    type="text"
                    id="owner"
                    placeholder="Nombre del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="owner"
                    @input="$emit('update:owner', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label
                    for="email"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Email del propietario
                </label>
                <input
                    type="email"
                    id="email"
                    placeholder="Email del propietario"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="email"
                    @input="$emit('update:email', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label
                    for="discharge"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Fecha de alta
                </label>
                <input
                    type="date"
                    id="discharge"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    :value="dischargeDate"
                    @input="$emit('update:discharge-date', $event.target.value)"
                >
            </div>
            <div class="mb-5">
                <label
                    for="symptoms"
                    class="block text-gray-700 uppercase font-bold"
                >
                    Síntomas
                </label>
                <textarea
                    type="date"
                    id="symptoms"
                    placeholder="Describe los síntomas"
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                    :value="symptoms"
                    @input="$emit('update:symptoms', $event.target.value)"
                ></textarea>
            </div>

            <input
                type="submit"
                :value="[editing ? 'Guardar cambios' : 'Registrar paciente']"
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
            >
        </form>
    </div>
</template>