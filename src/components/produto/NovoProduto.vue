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
                  :items="categoriasItems"
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
                  :items="fornecedoresItems"
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
                  v-model="fields.qtdEstoque"
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
                  @input="(value) => change('porcentagem', value)"
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
     <v-snackbar
      v-model="snackbar.visible"
      :timeout="snackbar.timeout"
      color="primary accent-4"
      elevation="24"
      right
    >
      {{ snackbar.message }}

      <template v-slot:action="{ attrs }">
        <v-btn
          color="white"
          text
          v-bind="attrs"
          @click="snackbar.visible = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
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
                nome: "Produto Teste",
                categoria: "CER",
                preco: "100.00",
                codigoBarras: "123412341234",
                fornecedor: "Bolhas e Bolhas",
                qtdEstoque: 1.015,
                urlImg: "https://cdn.vuetifyjs.com/images/cards/sunshine.jpg",
                promocao: false,
                dates: [],
                descricao: "Uma descrição qualquer =)",
                porcentagem: 0.15,
            },
            fornecedores: [],
            categorias: [],
            fornecedoresItems: [],
            categoriasItems: [],
            snackbar: { visible: false, message: "", timeout: 2500 }
        }
    },
    mounted() {
      const fornecedor_url = getApiURL() + 'fornecedor';
      axios.get(fornecedor_url).then(response => {
        const { data } = response;
        this.fornecedores = data;
        this.fornecedoresItems = Array.from(data, f => f.razaoSocial); 
      });

      const categoria_url = getApiURL() + 'categoria';
      axios.get(categoria_url).then(response => {
        const { data } = response;
        this.categorias = data;
        this.categoriasItems = Array.from(data, c => c.codigo); 
      });
    },
    methods: {
        change(field, e) {
            this.fields[field] = e;
        },   
        save() {
          const url = getApiURL() + 'produto';
          const { nome, preco, codigoBarras, urlImg, qtdEstoque, promocao, fornecedor, dates, porcentagem, categoria, descricao } = this.fields;

          const data = {
            nome: nome,
            codBarras: codigoBarras,
            preco: preco,
            qtdEstoque: qtdEstoque,
            fornecedor: this.fornecedores.find(f => f.razaoSocial == fornecedor),
            urlImg: urlImg,
            categoria: this.categorias.find(c => c.codigo == categoria),
            descricao: descricao,
          }

          if (promocao) {
            data.promocao = {
              dataInicial: dates[0],
              dataFinal: dates[1],
              porcentagem: porcentagem
            }
          }

          let newData = Object.assign({}, data);
          // Object.keys(data).map(item => {
          //   if (typeof(data[item]) == 'object') {
          //     if (data[item].id) delete data[item].id
          //     Object.keys(data[item]).map(o => {
          //       if (typeof(data[item][o]) == 'object') delete data[item][o].id;
          //     });
          //   }
          // })

          axios.post(url, newData).then(response => {
              console.log(response);
              this.dialog = false;
              this.snackbar = { visible: true, message: "Produto cadastrado com sucesso!", timeout: 2500 };
          }).then(() => {
            this.$router.push("produtos").catch(err => console.log(err));
            window.location.reload()
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