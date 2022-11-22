<template>
    <v-dialog max-width="94vw" class="main-foodservice-productsale">
        <template v-slot:activator="{ attrs, on }">
            <v-list-item
            color="primary"
            v-bind="attrs" 
            v-on="on" 
            @click="getProductSale">
                <v-list-item-icon>
                    <v-icon>mdi-plus</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    Produtos para Venda
                </v-list-item-content>
            </v-list-item>
        </template>
        <v-card class="grid-products">
            <ManageProductSale 
            @reset="reset" 
            @getProduct="getProduct" 
            @getProductSale="getProductSale" 
            @editDelProdLoadingTableItems="getProduct"
            @editProduct="editProduct"
            :loadingTableItems="loadingTableItems" 
            :prod="prod" 
            :editItem="editItem" 
            :edit="edit"></ManageProductSale>
            <ListProductSale @editProduct="editProduct" @deleteProduct="deleteProduct" :prodSale="prodSale"></ListProductSale>
        </v-card>
    </v-dialog>
</template>

<script>
import EditProductSale from "./actions/EditProductSale.vue";
import ManageProductSale from "./actions/ManageProductSale.vue";
import ListProductSale from "./actions/ListProductSale.vue";
export default {
    name: "Product",
    components: {
    EditProductSale,
    ManageProductSale,
    ListProductSale
},
    data () {
        return {
            loadingTableItems: false,
            loadingTableList: false,
            form: {},
            prod: [],
            prodSale: [],
            edit: false,
            editItem: {
                id: null,
                name: null,
                price: null,
                items: [],
            },
        }
    },
    methods: {
        reset () {
            this.edit = false
            this.editItem = {
                id: null,
                name: null,
                price: null,
                items: [],
            }
        },
        async editProduct (id) {
            this.loadingTableItems = true
            this.edit = true
            this.editItem = this.prodSale.filter((i) => { return i.id == id })[0]
            const req = await fetch(process.env.HOST_BACK+'/register/getProductItems/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                company_worker: localStorage.getItem('user_id'),
                product: this.editItem.id,
            }),{
                method: "GET",
            })
            if (req.status == 200) {
                const res = await req.json()
                this.editItem["items"] = res
                this.loadingTableItems = false
            }
        },
        async deleteProduct (id) {
            this.loadingTableList = true
            const req = await fetch(process.env.HOST_BACK+'/register/deleteProductSale/?',{
                method: "POST",
                body: JSON.stringify({
                    company: localStorage.getItem("company"),
                    token: localStorage.getItem('refresh'),
                    product: id,
                    type: 2
                }),
                headers: { "Content-Type": "application/json"}
            })
            if (req.status == 200) {
                this.getProductSale()
                this.loadingTableList = false
            }
        },
        async getProductSale () {
            this.loadingTableList = true
            const req = await fetch(process.env.HOST_BACK+'/register/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                company_worker: localStorage.getItem('user_id'),
                type: 2,
            }), {
                method: "GET",
            })
            if (req.status == 200) {
                const res = await req.json()
                this.prodSale = res
                this.loadingTableList = false
            }
        },
        async getProduct () {
            this.loadingTableItems = true
            const req = await fetch(process.env.HOST_BACK+'/register/getProduct/?'+new URLSearchParams({
                token: localStorage.getItem('refresh'),
                company: localStorage.getItem('company'),
                type: 1,
            }), {
                method: "GET",
            })
            if (req.status == 200) {
                const res = await req.json()
                this.prod = res
                this.loadingTableItems = false
            }
        },
    }
}
</script>

<style>
.main-foodservice-productsale {
    grid-area: productsale;

    padding: 1vh 1vh 0 0;
}
.grid-products {
    display: grid;
    grid-template-columns: 40% 60%;
    grid-template-rows: 90vh;
    grid-template-areas: 'newproduct product';
}
.list-products {
    grid-area: product;
}
</style>