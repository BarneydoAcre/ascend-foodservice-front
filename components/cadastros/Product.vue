<template>
    <v-dialog max-width="94vw" class="main-foodservice-product">
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" 
            @click="getProduct">
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Produtos para montagem
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card>
            <v-card-text>
                <AddProduct ref="addProduct" @getProduct="getProduct" @reset="reset" :form="form" :edit="edit"></AddProduct>
                <v-card-title>Editar produtos cadastrados</v-card-title>
                <v-data-table fixed-header dense height="37vh" :loading="loadingTable" :headers="headers" :items="products" :items-per-page="-1" hide-default-footer>
                    <template v-slot:item.actions="{ item }">
                        <v-icon small @click="editProduct(item)">mdi-pencil</v-icon>
                        <v-icon small @click="deleteProduct(item)">mdi-delete</v-icon>
                    </template>
                </v-data-table>
            </v-card-text>
        </v-card>
    </v-dialog>
</template>

<script>
import AddProduct from "./actions/AddProduct.vue";
export default {
    name: "Product",
    components: {
        AddProduct
    },
    data () {
        return {
            loadingTable: false,
            edit: false,
            form: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                token: localStorage.getItem("refresh"),
                type: 1,
                id: null,
                name: null,
                brand: null,
                measure: null,
                stock: null,
                cost: null,
            },
            products: [],
            headers: [
                { text: 'ID', justify: 'center', align: 'center', value: 'id' },
                { text: 'Produto', align: "center", justify: "center", value: 'name' },
                { text: 'Marca', align: "center", justify: "center", value: 'brand' },
                { text: 'Custo', align: "center", justify: "center", value: 'cost' },
                { text: 'Qtde.', align: "center", justify: "center", value: 'stock' },
                { text: 'Ações', align: "center", justify: "center", value: 'actions' },
            ]
        }
    },
    methods: {
        editProduct (i) {
            let prod = this.products.filter((p) => { return p.id == i.id })[0]

            this.form.id = prod.id
            this.form.name = prod.name
            this.form.brand = parseInt(prod.brand_id)
            this.form.measure = parseInt(prod.measure_id)
            this.form.stock = prod.stock
            this.form.cost = prod.cost
            
            this.edit = true
        },
        reset () {
            this.edit = false
        },
        async getProduct () {
            this.loadingTable = true
            const req = await fetch(process.env.HOST_BACK+'/register/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                type: 1,
            }), {
                method: "GET",
            })
            const res = await req.json()
            this.loadingTable = false
            this.products = res
        },
        async deleteProduct (item) {
            const req = await fetch(process.env.HOST_BACK+'/register/deleteProduct/', {
                method: "POST",
                body: JSON.stringify({
                    token: localStorage.getItem('refresh'),
                    company: localStorage.getItem('company'),
                    id: item.id,
                    type: item.type,
                }),
                headers: {'Content-Type': 'application/json'},
            })
            if (req.status == 200) {
                this.getProduct()
            }if (req.status == 201) {
                console.log('This product already in use')
            }
        }
    }
}
</script>

<style>
.main-foodservice-product {
    grid-area: product;

    padding: 1vh 1vh 1vh 0;
}
</style>