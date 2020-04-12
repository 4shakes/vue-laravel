<template>
    <div>
        <h3>Agregar Tareas</h3>
        <form @submit.prevent="agregar">
            <input
                type="text"
                placeholder="Nombre"
                class="form-control mb-2"
                v-model="tarea.nombre"
            />
            <input
                type="text"
                placeholder="Descripcion"
                class="form-control mb-2"
                v-model="tarea.descripcion"
            />
            <button class="btn btn-block btn-success" type="submit">
                Enviar
            </button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tareas: [],
            tarea: {
                nombre: "",
                descripcion: ""
            }
        };
    },
    methods: {
        agregar() {
            console.log(this.tarea.nombre, this.tarea.descripcion);
        }
    }
};
</script>

<style></style>
