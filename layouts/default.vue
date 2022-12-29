<template>
    <v-app>
        <v-navigation-drawer v-if="logged" permanent mini-variant expand-on-hover fixed app>
            <v-list-item>
                <v-list-item-content>
                    <v-list-item-title class="text-h6">
                        {{ user.username }}
                    </v-list-item-title>
                    <v-list-item-subtitle>
                        {{ $route.params.company }}
                    </v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list dense nav v-if="$route.params.company">
                <v-list-item v-for="item in items" :key="item.title" :disabled="item.disabled" :to="'/'+$route.params.company+'/'+item.link">
                    <v-list-item-icon>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
            
            <template v-slot:append>
                <v-list dense nav v-if="$route.params.company">
                    <v-list-item link :to="'/'+$route.params.company+'/settings/'">
                        <v-list-item-icon>
                            <v-icon>mdi-cog</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Configurações</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
                <v-list dense nav>
                    <v-list-item @click="logout" link>
                        <v-list-item-icon>
                            <v-icon>mdi-logout</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Sair</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                <!-- ThemeLightDark -->
                    <v-list-item v-if="$vuetify.theme.dark == true" @click="$vuetify.theme.dark = false">
                        <v-list-item-icon>
                            <v-icon>mdi-theme-light-dark</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Modo Claro/Escuro</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-list-item v-else @click="$vuetify.theme.dark = true">
                        <v-list-item-icon>
                            <v-icon>mdi-theme-light-dark</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Modo Claro/Escuro</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
            </template>
        </v-navigation-drawer>
        <v-main v-if="logged">
            <v-container>
                <Nuxt />
            </v-container>
        </v-main>
        <Nuxt v-if="!logged && $route.path == '/'" />
        <v-speed-dial
        class="v-btn-fab"
        :transition="'scale-transition'">
            <template v-slot:activator>
                <BugReport></BugReport>
            </template>
        </v-speed-dial>
    </v-app>
</template>

<script>
import BugReport from '../components/default/BugReport.vue'
export default {
    name: "DefaultLayout",
    components: {
        BugReport,
    },
    data() {
        return {
            items: [
                { title: "Dashboard (Em Breve)", icon: "mdi-view-dashboard", link: "dashboard/", disabled: false },
                { title: "Caixa", icon: "mdi-point-of-sale", link: "caixa/", disabled: false },
                { title: "Vendas", icon: "mdi-cash-plus", link: "vendas/", disabled: false },
                { title: "Compras (Em Breve)", icon: "mdi-cart-plus", link: "compras/", disabled: true },
                { title: "Contas à Receber (Em Breve)", icon: "mdi-bank-transfer-in", link: "contas-receber/", disabled: false },
                { title: "Contas à Pagar (Em Breve)", icon: "mdi-bank-transfer-out", link: "contas-pagar/", disabled: false },
                { title: "Cadastros", icon: "mdi-plus", link: "cadastros/", disabled: false },
                { title: "Ajuda (Em Breve)", icon: "mdi-help-box", link: "help/", disabled: true },
            ],
            user: {
                username: '',
            },
            logged: false,
            clipped: false,
            drawer: false,
            fixed: false,
            miniVariant: false,
            right: true,
            rightDrawer: false,
        };
    },
    mounted () {
        // this.$vuetify.theme.isDark = false;
        this.user.username = localStorage.getItem('first_name')
        this.verifyLogin()
        // console.log(this.$route)
    },
    methods: {
        logout() {
            localStorage.setItem("token", "")
            localStorage.setItem('user_id', '')
            localStorage.setItem('username', '')
            localStorage.setItem('email', '')
            localStorage.setItem('first_name', '')
            localStorage.setItem('last_name', '')
            this.logged = false;
            this.verifyLogin()
        },
        async verifyLogin() {
            const req = await fetch(process.env.HOST_BACK+'/accounts/auth/', {
                method: 'GET',
                headers: new Headers({
                    "Authorization": "Token "+localStorage.getItem("token")
                })
            })
            if (req.status == 200) {
                const res = await req.json()
                localStorage.setItem("token", res[0].key)
                localStorage.setItem('user_id', res[0].user.id)
                localStorage.setItem('username', res[0].user.username)
                localStorage.setItem('email', res[0].user.email)
                localStorage.setItem('first_name', res[0].user.first_name)
                localStorage.setItem('last_name', res[0].user.last_name)
                this.logged = true;
            }
            if (req.status == 401 || req.status == 403 && window.location. href != process.env.HOST_FRONT+'/#login-form' && window.location. href != process.env.HOST_FRONT+'/#company-form') {
                localStorage.clear()
                this.logged = false;
                window.location.href = process.env.HOST_FRONT
            }
        },
    },
}
</script>

<style>
.v-btn-fab {
    position: absolute;
    bottom: 0;
    right: 0;
    margin: 0 2vw 4vh 0;
}
</style>