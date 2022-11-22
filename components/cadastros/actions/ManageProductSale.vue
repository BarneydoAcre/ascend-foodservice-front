<template>
    <div>
        <v-card-title>Componentes do Produto</v-card-title>
        <v-card-text v-if="!edit">
            <v-form v-model="valid" ref="form" lazy-validation>
                <v-row dense>
                    <v-col cols="9">
                        <v-text-field dense filled label="Nome do Produto" v-model="editItem.name"></v-text-field>
                    </v-col>
                    <v-col cols="3">
                        <v-text-field dense filled label="Preço" v-model="editItem.price"></v-text-field>
                    </v-col>
                    <v-col cols="12">
                        <h3>Componentes</h3>
                    </v-col>
                    <v-col cols="4">
                        <v-select dense filled label="Componente" v-model="component" @focus="$emit('getProduct')" :items="prod" item-text="name" item-value="id"></v-select>
                    </v-col>
                    <v-col cols="3">
                        <v-text-field dense filled label="Qtde." v-model="quantity"></v-text-field>
                    </v-col>
                    <v-col cols="5" class="d-flex justify-space-between">
                        <v-btn fab small color="primary" @click="addItem">
                            <v-icon>mdi-plus</v-icon>
                        </v-btn>
                        <v-btn fab small color="warning" @click="reset">
                            <v-icon>mdi-close</v-icon>
                        </v-btn>
                        <v-btn fab small color="success" @click="addProductSale">
                            <v-icon>mdi-check</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-form>
            <v-data-table
            dense 
            height="45vh"
            class="elevation-4"
            :loading="loadingTableItems"
            :headers="headers" 
            :items-per-page="-1" 
            :items="editItem.items" 
            hide-default-footer>
                <template v-slot:item.actions="{ item }">
                    <v-icon small @click="delItem(item)">mdi-delete</v-icon>
                </template>
            </v-data-table>
        </v-card-text>
        <v-card-text v-else>
            <v-form v-model="valid" ref="form" lazy-validation>
                <v-row dense>
                    <v-col cols="9">
                        <v-text-field dense filled label="Nome do Produto" @blur="editNamePriceProd" v-model="editItem.name"></v-text-field>
                    </v-col>
                    <v-col cols="3">
                        <v-text-field dense filled label="Preço" @blur="editNamePriceProd" v-model="editItem.price"></v-text-field>
                    </v-col>
                    <v-col cols="12">
                        <h3>Componentes</h3>
                    </v-col>
                    <v-col cols="6">
                        <v-select dense filled label="Componente" v-model="component" @focus="$emit('getProduct')" :items="prod" item-text="name" item-value="id"></v-select>
                    </v-col>
                    <v-col cols="3">
                        <v-text-field dense filled label="Qtde." v-model="quantity"></v-text-field>
                    </v-col>
                    <v-col cols="3" class="d-flex justify-space-between">
                        <v-btn fab small color="primary" @click="editAddItem">
                            <v-icon>mdi-plus</v-icon>
                        </v-btn>
                        <v-btn fab small color="warning" @click="reset">
                            <v-icon>mdi-close</v-icon>
                        </v-btn>
                    </v-col>
                </v-row>
            </v-form>
            <v-data-table
            dense 
            height="45vh"
            class="elevation-4"
            :loading="loadingTableItems"
            :headers="headers" 
            :items-per-page="-1" 
            :items="editItem.items" 
            hide-default-footer>
                <template v-slot:item.actions="{ item }">
                    <v-icon small @click="editDelItem(item)">mdi-delete</v-icon>
                </template>
            </v-data-table>
        </v-card-text>
    </div>
</template>

<script>
export default {
    name: "ManageProductSale",
    emits: [
        "getProductSale", 
        "getProduct", 
        "reset",
        "editDelProdLoadingTableItems",
        "editProduct", 
    ],
    props: [
        "prod", 
        "editItem",
        "editProdItems", 
        "loadingTableItems", 
        "edit"
    ],
    data () {
        return {
            valid: false,
            component: null,
            quantity: null,
            formProductItems: {
                company: localStorage.getItem("company"),
                company_worker: localStorage.getItem("user_id"),
                product_sale: null,
                items: [],
            },
            dialog: false,
            headers: [
                { 
                    text: "ID",
                    align: "center",
                    justify: "center",
                    value: "id"
                },
                { text: "Produto", value: "name" },
                { text: "Qtde.", value: "quantity" },
                { text: "Preço", value: "cost" },
                { text: "Ações", value: "actions" },
            ],
        }
    },
    mounted () {
    },
    methods: {
        reset () {
            this.$emit('reset')
        },
        async addProductSale () {
            const req = await fetch(process.env.HOST_BACK+"/register/addProduct/", {
                method: "POST",
                body: JSON.stringify({
                    company: localStorage.getItem("company"),
                    company_worker: localStorage.getItem("user_id"),
                    name: this.editItem.name,
                    price: this.editItem.price,
                    type: 2,
                }),
                headers: { "Content-Type": "application/json"}
            })
            if (req.status == 200) {
                const res = await req.json()
                this.editItem["product_sale"] = res
                this.addProductSaleItems()
            }
        },
        async addProductSaleItems () {
            const req = await fetch(process.env.HOST_BACK+"/register/addProductItems/", {
                method: "POST",
                body: JSON.stringify({
                    company: localStorage.getItem("company"),
                    company_worker: localStorage.getItem("user_id"),
                    product_sale: this.editItem.product_sale,
                    items: this.editItem.items,
                }),
                headers: { "Content-Type": "application/json"}
            })
            if (req.status == 200) {
                this.dialog = false
                this.editItem.items = []
                this.reset()
                this.$emit('getProductSale')
            }
        },
        addItem () {
            let objItem = this.prod.filter((i) => {return i.id == this.component})[0]
            if (this.editItem.items.filter((i) => {return i.id == this.component}).length == 0) {
                objItem["quantity"] = this.quantity
            }else {
                let item = this.editItem.items.filter((i) => {return i.id == this.component})[0]
                console.log(item)
                objItem["quantity"] = parseFloat(item.quantity) + parseFloat(this.quantity)
                this.editItem.items = this.editItem.items.filter((i) => {return i.id != this.component})
            }
            this.editItem.items.push(objItem)
            this.component = null
            this.quantity = null
        },
        delItem (item) {
            this.editItem.items = this.editItem.items.filter((i) => {return i.id != item.id})
        },

        async editAddItem () {
            let quantity = 0
            if (this.editItem.items.filter((i) => {return i.id == this.component}).length == 0) {
                quantity = this.quantity
            }else {
                quantity = parseFloat(this.editItem.items.filter((i) => {return i.id == this.component})[0].quantity) + parseFloat(this.quantity)
            }
            const req = await fetch(process.env.HOST_BACK+'/register/addProductItems/', {
                method: "POST",
                body: JSON.stringify({
                    company: localStorage.getItem("company"),
                    company_worker: localStorage.getItem("user_id"),
                    product_sale: this.editItem.id,
                    items: [
                        {
                            id: this.component,
                            quantity: quantity,
                        },
                    ],
                }),
                headers: { "Content-Type": "application/json" },
            })
            if (req.status == 200) {
                this.$emit('editProduct', this.editItem.id)
                this.component = null
                this.quantity = null
            }
        },
        async editDelItem (item) {
            this.$emit("editDelProdLoadingTableItems")
            const req = await fetch(process.env.HOST_BACK+'/register/deleteProductSale/', {
                method: "POST",
                body: JSON.stringify({
                    company: localStorage.getItem("company"),
                    token: localStorage.getItem('refresh'),
                    type: 1,
                    product: item.prod,
                    product_item: item.prod_item
                }),
                headers: { "content-type": "application/json" },
            })
            if (req.status == 200) {
                this.editItem.items = this.editItem.items.filter((i) => {return i.id != item.id})
            }
            this.$emit("editDelProdLoadingTableItems")
        },
        async editNamePriceProd () {
            let data = JSON.stringify({
                company: localStorage.getItem("company"),
                token: localStorage.getItem("refresh"),
                id: this.editItem.id,
                name: this.editItem.name,
                price: this.editItem.price,
                type: 2,
            })
            const req = await fetch(process.env.HOST_BACK+'/register/editProduct/',{
                method: "POST",
                body: data,
                headers: { "Content-Type": "application/json" },
            })
            if (req.status == 200) {

            }
        },
    }
}
</script>

<style>
.main-addproduct {
    grid-area: newproduct;
}
</style>