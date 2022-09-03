<template>
    <div class="container">
        <div class="justify-content-center">
            <h1 class="text-center"><b>Agenda de Contactos</b></h1>
            <hr>
            <!-- Button trigger modal -->
            <button @click="update=false; openModal();" type="button" class="btn btn-primary">
            Nuevo Contacto
            </button>

            <!-- Modal -->
            <div class="modal" :class="{show:modal}">
                <div class="modal-dialog">
                    <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">{{titleModal}}</h5>
                        <button @click="closeModal();" type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div>
                            <label for="name">Nombre</label>
                            <input v-model="contacto.name" type="text" class="form-control" id="name" name="name" placeholder="Nombre del Contacto">
                        </div>
                        <div>
                            <label for="last_name">Apellidos</label>
                            <input v-model="contacto.last_name" type="text" class="form-control" id="last_name" name="last_name" placeholder="Apellido(s) del Contacto">
                        </div>
                        <div>
                            <label for="email">Email</label>
                            <input v-model="contacto.email" type="text" class="form-control" id="email" name="email" placeholder="Correo Electrónico">
                        </div>
                        <div>
                            <label for="phone">Teléfono</label>
                            <input v-model="contacto.phone" type="number" class="form-control" id="phone" name="phone" placeholder="Teléfono">
                        </div>
                        <div>
                            <label for="address">Dirección</label>
                            <input v-model="contacto.address" type="text" class="form-control" id="address" name="address" placeholder="Dirección">
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button  @click="closeModal();" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
                        <button @click=save(); type="button" class="btn btn-success">Guardar Cambios</button>
                    </div>
                    </div>
                </div>
            </div>
            <br>
            <br>
            <table class="table table-striped table-hover rounded">
                <thead class="table-dark">
                    <tr>
                        <th scope="col" class="text-center">Id</th>
                        <th scope="col" class="text-center">Nombre</th>
                        <th scope="col" class="text-center">Apellidos</th>
                        <th scope="col" class="text-center">Email</th>
                        <th scope="col" class="text-center">Teléfono</th>
                        <th scope="col" class="text-center">Dirección</th>
                        <th scope="col" colspan="2" class="text-center">Acción</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="contact in people" :key="contact.id">
                        <th scope="row">{{contact.id}}</th>
                        <td>{{ contact.name }}</td>
                        <td>{{ contact.last_name }}</td>
                        <td>{{ contact.email }}</td>
                        <td>{{ contact.phone }}</td>
                        <td>{{ contact.address }}</td>
                        <td>
                            <button @click="update=true; openModal(contacto);" class="btn btn-warning">Editar</button>
                        </td>
                        <td>
                            <button @click="remove(contact.id)" class="btn btn-danger">Eliminar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

    export default {
        data() {
            return {
                contacto: {
                    name: '',
                    last_name: '',
                    email: '',
                    phone: '',
                    address: '',
                },
                id:0,
                update: true,
                modal:0,
                titleModal: '',
                people: [],
            }
        },
        methods: {
            async list() {
                const res = await axios.get('contacts')
                this.people = res.data
            },
            async remove(id) {
                const res = await axios.delete('/contacts/' +id);
                this.list();
            },
            async save(id) {
                if (this.update) {
                    const res = await axios.put('/contacts', +this.id, this.contacto)
                }else{
                    const res  = await axios.post('/contacts', this.contacto);
                }
                this.closeModal();
                this.list();
            },
            openModal(data={}) {
                this.modal=1;
                if (this.update){
                    this.id = data.id,
                    this.titleModal="Modificar Contacto";
                    this.contacto.name = data.name;
                    this.contacto.last_name = data.last_name;
                    this.contacto.email = data.email;
                    this.contacto.phone = data.phone;
                    this.contacto.address = data.address;
                }else{
                    this.id = 0,
                    this.titleModal="Crear Contacto";
                    this.contacto.name = '';
                    this.contacto.last_name = '';
                    this.contacto.email = '';
                    this.contacto.phone = '';
                    this.contacto.address = '';
                }
            },
            closeModal() {
                this.modal=0;
            },
        },
        created () {
            this.list()
        },
    }
</script>

<style>
    .show{
        display: list-item;
        opacity: 1;
        background: rgba(44, 38, 75, 0.849);
    }
</style>
