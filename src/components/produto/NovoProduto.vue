<template>
  <v-row justify="center">
    <v-dialog
      v-model="dialog"
      @click:outside="close()"
      max-width="800px"
    >     
      <v-card>
        <v-card-title>
          <span class="headline">Novo Produto</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                cols="12"
                sm="4"
                md="4"
              >
                <v-text-field
                  prepend-icon="mdi-rename-box"
                  label="Nome*"
                  name="nome*"
                  v-model="fields.nome"
                  required
                  @input="(value) => change('nome', value)"
                ></v-text-field>
              </v-col>
              
              <v-col
                cols="12"
                sm="4"
                md="4"
              >
                <v-select
                  :items="['Categoria 1', 'Categoria 2']"
                  label="Categoria*"
                  name="categoria"
                  v-model="fields.categoria"
                  required
                  @change="(value) => change('categoria', value)"
                ></v-select>
              </v-col>
              <v-col
                cols="12"
                sm="4"
                md="4"
              >
                <v-text-field
                  label="Preço*"
                  name="preco"
                  prefix="R$"
                  prepend-icon="mdi-cash"
                  v-model="fields.preco"
                  required
                  type="number"
                  @input="(value) => change('preco', value)"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="4" md="4">
                <v-text-field
                  prepend-icon="mdi-barcode"
                  label="Código de Barras*"
                  v-model="fields.codigoBarras"
                  required
                  name="codigoBarras"
                  @input="(value) => change('codigoBarras', value)"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="4"
                md="4"
              >
                <v-select
                  prepend-icon="mdi-truck"
                  :items="fornecedores"
                  label="Fornecedor*"
                  v-model="fields.fornecedor"
                  required
                  name="fornecedor"
                  @change="(value) => change('fornecedor', value)"
                ></v-select>
              </v-col>
               <v-col
                cols="12"
                sm="4"
                md="4"
              >
                <v-text-field
                  prepend-icon="mdi-archive"
                  label="Quantidade em Estoque"
                  v-model="fields.estoque"
                  name="estoque"
                  @input="(value) => change('estoque', value)"
                  type="number"
                ></v-text-field>
              </v-col>
              <v-col
                cols="12"
                sm="5"
                md="5"
              >
                <v-text-field
                  label="Imagem (URL)"
                  v-model="fields.urlImg"
                  prepend-icon="mdi-image-area"
                  name="urlImg"
                  @input="(value) => change('urlImg', value)"
                ></v-text-field>
              </v-col>
              
              
              <v-col cols="12" md="7" sm="7">
                 <v-radio-group v-model="fields.promocao" mandatory row @change="(value) => change('promocao', value)">
                    <template v-slot:label>
                        <div class="text-md-subtitle-1">Produto contém promoção?</div>
                    </template>
                     <v-radio :value="false">
                        <template v-slot:label>
                        <div>Não</div>
                        </template>
                    </v-radio>
                    <v-radio :value="true">
                        <template v-slot:label>
                        <div>Sim</div>
                        </template>
                    </v-radio>
                </v-radio-group>                 
              </v-col>   
              <v-col
                cols="12"
                sm="6"
                md="6"
                v-if="fields.promocao"
              >
                <v-dialog
                  ref="dialog"
                  v-model="modal"
                  :return-value.sync="fields.dates"
                  persistent
                  width="290px"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="dateRangeText"
                      label="Período da Promoção"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    range
                    v-model="fields.dates"
                    scrollable
                  >
                    <v-spacer></v-spacer>
                    <v-btn
                      text
                      color="primary"
                      @click="modal = false"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.dialog.save(fields.dates)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-dialog>
              </v-col>
              <v-col cols="12" md="6" sm="6" v-if="fields.promocao">
                <v-text-field
                  label="Porcentagem"
                  name="porcentagem"
                  type="number"
                  prepend-icon="mdi-percent"
                ></v-text-field>
              </v-col>

               <v-col cols="12">
                <v-textarea
                    prepend-icon="mdi-comment-text"
                    clearable
                    clear-icon="mdi-close-circle"
                    label="Descrição"
                    counter
                    rows="2"
                    auto-grow
                    :value="fields.descricao"
                    @input="(value) => change('descricao', value)"
                ></v-textarea>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="close()"
          >
            Close
          </v-btn>
          <v-btn
            color="primary"
            text
            @click="save()"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import { getApiURL } from '../../utils/utils';
import axios from 'axios';

export default {
    name: "NovoProduto",
    props: ["dialog", "close"],
    data() {
        return {
            modal: false,
            fields: {
                nome: "",
                categoria: "",
                preco: "",
                codigoBarras: "",
                fornecedor: "",
                qtdEstoque: "",
                urlImg: "",
                promocao: false,
                dates: [],
                descricao: "",
            },
            fornecedores: [],

        }
    },
    mounted() {
      const url = getApiURL() + 'fornecedor/all';
      axios.get(url).then(response => {
        const { data } = response;
        this.fornecedores = Array.from(data, f => f.razaoSocial); 
      });
    },
    methods: {
        change(field, e) {
            this.fields[field] = e;
        },
        save() {
          const url = getApiURL() + '/produto/registrar';
          const { nome, categoria, preco, codigoBarras, fornecedor,  } = this.fields;


          const fields = {
            nome, 
            categoria,
            preco,
            codigoBarras, 
            fornecedor
          }

          axios.post(url, fields).then(response => {
              console.log(response);
          })
        },
        
    },
    computed: {
      dateRangeText () {
        return this.fields.dates.join(' ~ ')
      },
    },
}
</script>