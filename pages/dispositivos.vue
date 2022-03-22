<template>
    <div>
        <!-- Formulario -->
        <div class="row">
            <card>
                <div slot="header">
                    <h4 class="card-title">Agregar dispositivo</h4>
                </div>
                
                <div class="row">
                    <div class="col-4">
                        <base-input
                        label="Nombre"
                        type="text"
                        placeholder="Ej: Casa, Oficina..."
                        >
                        </base-input>
                    </div>

                    <div class="col-4">
                        <base-input
                        label="Id"
                        type="text"
                        placeholder="Ej: 7777-8888"
                        >
                        </base-input>
                    </div>

                    <div class="col-4">
                        <slot name="label">
                        <label> Plantilla </label>
                        </slot>

                        <el-select
                            value="1"
                            placeholder="Plantilla"
                            class="select-primary"
                            style="width:100%"
                        >
                            <el-option
                                class="text-dark"
                                value="Plantilla 1"
                                label="Plantilla 1"
                            ></el-option>

                            <el-option
                                class="text-dark"
                                value="Plantilla 2"
                                label="Plantilla 2"
                            ></el-option>

                            <el-option
                                class="text-dark"
                                value="Plantilla 3"
                                label="Plantilla 3"
                            ></el-option>
                        </el-select>
                    </div>
                </div>

                <div class="row pull-right">
                    <div class="col-12">
                        <base-button type="primary" class="mb-3" size="lg">Agregar</base-button>
                    </div>
                </div>
            </card>
        </div>

        <!-- Listado de dispositivos -->
        <div class="row">
            <card>
                <div slot="header">
                    <h4 class="card-title">Dispositivos</h4>
                </div>

                <el-table :data="dispositivos">
                    <el-table-column label="Nro." min-width="30" align="left">
                        <div slot-scope="{row, $index}" align="right">
                            {{$index + 1}}
                        </div>
                    </el-table-column>
                    <el-table-column prop="nombre" label="Nombre"></el-table-column>
                    <el-table-column prop="dId" label="Id"></el-table-column>
                    <el-table-column prop="nombrePlantilla" label="Plantilla"></el-table-column>
                    <el-table-column label="Acciones">
                        <div slot-scope="{row, $index}">
                            <el-tooltip content="Almacenar en base de datos">
                                <base-switch :value="row.reglaActualizador" type="primary" onText="Si" offText="No" @click="actualizarEstadoReglaAlmacenador($index)"></base-switch>
                            </el-tooltip>

                            <el-tooltip
                                content="Borrar"
                                effect="light"
                                :open-delay="300"
                                placement="top"
                            >
                                <base-button type="danger" icon size="sm" class="btn-link" @click="borrarDispositivo(index)">
                                    <i class="tim-icons icon-simple-remove"></i>
                                </base-button>
                            </el-tooltip>
                        </div>
                    </el-table-column>
                </el-table>
            </card>
        </div>

        <Json :value="dispositivos"></Json>
    </div>
</template>

<script>
    import { Table, TableColumn } from "element-ui"
    import { Select, Option } from "element-ui"

    export default {
        components: {
            [Table.name]: Table,
            [TableColumn.name]: TableColumn,
            [Option.name]: Option,
            [Select.name]: Select
        },
        data() {
            return {
                dispositivos: [
                    {
                        nombre: "Casa",
                        dId: "8888",
                        nombrePlantilla: "Power Sensor",
                        pId: "984237562348756ldksjfh",
                        reglaActualizador: false
                    },
                    {
                        nombre: "Oficina",
                        dId: "1111",
                        nombrePlantilla: "Power Sensor",
                        pId: "984237562348756ldksjfh",
                        reglaActualizador: true
                    },
                    {
                        nombre: "Granja",
                        dId: "99999",
                        nombrePlantilla: "Power Sensor",
                        pId: "984237562348756ldksjfh",
                        reglaActualizador: false
                    }
                ]
            }
        },
        methods: {
            borrarDispositivo(dispositivo) {
                alert(`Borrando ${dispositivo.nombre}`)
            },
            actualizarEstadoReglaAlmacenador(index) {
                this.dispositivos[index].reglaActualizador = !this.dispositivos[index].reglaActualizador
            }
        }
    };
</script>
