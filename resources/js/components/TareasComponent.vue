<template>
    <div>
        <form @submit.prevent="editarNota(nota)" v-if="editarActivo">
            <h3>Editar Tarea</h3>
            <input
                type="text"
                placeholder="Nombre"
                class="form-control mb-2"
                v-model="nota.nombre"
            />
            <input
                type="text"
                placeholder="Descripcion"
                class="form-control mb-2"
                v-model="nota.descripcion"
            />
            <button class="btn btn-block btn-success" type="submit">
                Guardar
            </button>
            <button
                class="btn btn-block btn-danger"
                type="submit"
                @click="cancelarEdicion()"
            >
                Cancelar Edicion
            </button>
        </form>
        <form @submit.prevent="agregar" v-else>
            <h3>Agregar Notas</h3>
            <input
                type="text"
                placeholder="Nombre"
                class="form-control mb-2"
                v-model="nota.nombre"
            />
            <input
                type="text"
                placeholder="Descripcion"
                class="form-control mb-2"
                v-model="nota.descripcion"
            />
            <button class="btn btn-block btn-success" type="submit">
                Enviar
            </button>
        </form>
        <hr class="mt-3" />
        <h3>Listado de notas</h3>
        <ul class="list-group my-2">
            <li
                class="list-group-item"
                v-for="(item, index) in notas"
                :key="index"
            >
                <span class="badge badge-primary float-right">{{
                    item.updated_at
                }}</span>
                <p>{{ item.nombre }}</p>
                <p>{{ item.descripcion }}</p>
                <button
                    class="btn btn-warning btn-sm"
                    @click="editarFormulario(item)"
                >
                    Editar
                </button>
                <button
                    class="btn btn-danger btn-sm"
                    @click="eliminarNota(item, index)"
                >
                    Eliminar
                </button>
            </li>
        </ul>
    </div>
</template>

<script>
const URLactual = "http://localhost:8080/vue-laravel/public";
export default {
    data() {
        return {
            notas: [],
            nota: {
                nombre: "",
                descripcion: ""
            },
            editarActivo: false
        };
    },
    created() {
        axios.get(URLactual + "/notas").then(res => {
            this.notas = res.data;
        });
    },
    methods: {
        agregar() {
            if (
                /* trim('  a  '): elimina los espacios en blanco */
                this.nota.nombre.trim() === "" ||
                this.nota.descripcion.trim() === ""
            ) {
                alert("Debes completar todos los campos antes de guardar");
                return;
            }
            /* console.log(this.nota.nombre, this.nota.descripcion); */

            const params = {
                nombre: this.nota.nombre,
                descripcion: this.nota.descripcion
            };
            this.nota.nombre = "";
            this.nota.descripcion = "";
            /* url del web.php */

            axios.post(URLactual + "/notas", params).then(res => {
                /*  console.log(res.data); */
                this.notas.push(res.data);
            });
        },
        eliminarNota(item) {
            axios.delete(`${URLactual}/notas/${item.id}`).then(() => {
                this.notas.splice(index, 1);
            });
        },
        editarFormulario(item) {
            this.editarActivo = true;
            this.nota.nombre = item.nombre;
            this.nota.descripcion = item.descripcion;
            this.nota.id = item.id;
        },
        editarNota(item) {
            const params = {
                nombre: item.nombre,
                descripcion: item.descripcion
            };
            this.nota = {
                nombre: "",
                descripcion: ""
            };
            this.editarActivo = false;
            axios.put(`${URLactual}/notas/${item.id}`, params).then(res => {
                const index = this.notas.findIndex(
                    notaBuscar => notaBuscar.id === res.data.id
                );
                this.notas[index].nombre = res.data.nombre;
                this.notas[index].descripcion = res.data.descripcion;
            });
        },
        cancelarEdicion() {
            this.editarActivo = false;
            this.nota = {
                nombre: "",
                descripcion: ""
            };
        }
    }
};
</script>

<style></style>
