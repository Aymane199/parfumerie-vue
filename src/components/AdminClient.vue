<template>
    <div class="container">
        <b-row>
                        <b-col>
                            <b-button v-b-toggle.collapse-1 class="btn btn-outline-default" style="width: 100%;"><user-plus-icon size="1.5x" class="custom-class"></user-plus-icon> Ajouter un nouveau client</b-button>
                        </b-col>
        </b-row>
        <div class="row justify-content-center">
            <div class="col-md-10">
                <div class="card">
                    <div class="card-body">
                        <form>
                            <table class="table">
                            <thead>
                            <tr>
                                <th scope="col">Produit</th>
                                <th scope="col">Libellé</th>
                                <th scope="col">Type</th>
                                <th scope="col">Volume</th>
                                <th scope="col">Quantité</th>
                                <th scope="col">Prix</th>
                                <th scope="col">Actions</th>
                                <th scope="col">Autre</th>
                            </tr>
                            </thead>
                            <tbody>
                                <tr v-for="produit in allProduits" :key="produit.num_produit">
                                   <div class="card">
                                        <td> <img :v-bind:src="link"/></td>
                                        <td>{{produit.libelle_produit}}</td>
                                        <td>{{produit.type}}</td>
                                        <td>{{produit.attributes_produit.volume}}</td>
                                        <td>{{produit.totale}}</td>
                                        <td>{{produit.prixu_produit}}</td>
                                        <td>
                                            <button type="button" class="btn btn-cart" @click="Modify()">Modifier</button>
                                            <button type="button" class="btn btn-danger" @click="Supprimer(produit)">Supprimmer</button>
                                        </td>
                                        <td>
                                            <button type="button" @click="infosTraitement(produit)"><img src="../../public/images/dropdown.png" width="50" height="50"/></button>
                                        </td>
                                    </div>
                                </tr>
                            </tbody>
                            </table>
                        </form>
                         <product-modal v-if="clicked"></product-modal>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

 import ProductModal from "./ProductModal";

export default {
    name: "AdminClient",
    components: {
            ProductModal
},
data() {
            return {
                currentId: null,
                images: [],
                allProduits: [], // Main table of products
                clicked: false,
                link: null
            }
        },
        methods: {
           
            async fetchallProduits() {
                this.loading = true;
                let returnPromise;
                await this.axios.get('/produit/all').then(({data}) => {
                    this.allProduits = data;
                    returnPromise = data;
                    this.loading = false;
                });
                return returnPromise
            },
            
            parseImage() {
                let images = this.product.attributes_produit.image;
                images.forEach(image => {
                    let obj = {
                        link: image,
                        thumbnail: image,
                    };
                    this.images.push(obj);
                });
            },
            Modify(){
                this.clicked=true;      //  Affiche la modal de modification
                this.$noty.success("Le produit a bien été modifié.", {layout: 'center'});
            },
            Supprimer(produit){
                this.allProduits= this.allProduits.splice(produit.num_produit,1);
                this.$noty.success("Le produit a bien été supprimé.", {layout: 'center'});
            }
        }
    }
</script>