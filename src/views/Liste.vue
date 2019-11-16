<template>
  <v-data-table
    :headers="headers"
    :items="Etudiants"
    :search="search"
    sort-by="dessert"
    class="my-5 elevation-5"
    hide-default-footer
     style="border-radius:25px;"
  >
    <template v-slot:top>
      <v-toolbar   color="white" style="border-radius:20px;">
        <v-toolbar-title>Etudiants</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-text-field
        v-model="search"
        append-icon="mdi-account-search"
        label="Rechercher"
        single-line
        hide-details
      ></v-text-field>
        <v-spacer></v-spacer>
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-133" v-on="on" >Ajouter
               <v-icon right dark>mdi-account-plus</v-icon>
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-form v-model="validd"
      :lazy-validation="lazy">
                <v-row ref="form">
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Nom" label="Nom" :rules="nameRules"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Prénom" label="Prénom" :rules="prenomRules"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Email" label="Email" :rules="emailRules"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Groupe" label="Groupe" :rules="groupeRules"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Date_de_naissance" label="Date de naissance" :rules="groupeRules"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Matricule" label="Matricule" :rules="groupeRules"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Adresse" label="Adresse" :rules="groupeRules"></v-text-field>
                  </v-col>
                </v-row>
                </v-form>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-3" text @click="close">Fermer</v-btn>
              <v-btn color="blue darken-3" :disabled="!validd" text @click="save">Sauvegarder</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

    <v-dialog v-model="dialogg"  max-width="600px">
      <v-card>
        <v-card-title>
          <span class="headline">Information étudiant</span>
        </v-card-title>
        <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Nom" label="Nom" :disabled="true"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Prénom" label="Prénom" :disabled="true"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Groupe" label="Groupe" :disabled="true"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Email" label="Email" :disabled="true"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Date_de_naissance" label="Date de naissance" :disabled="true"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Matricule" label="Matricule" :disabled="true"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.Adresse" label="Adresse" :disabled="true"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialogg = false">Fermer</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  
      </v-toolbar>
    </template>
    <template v-slot:item.action="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-account-edit
      </v-icon>
      <v-icon
        small
        class="mr-2"
        @click="showitem(item)"
      >
        mdi-account-card-details
      </v-icon>
      <v-icon
        small
        class="mr-2"
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
      
    </template>
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>
</template>


<script>
  export default {
    data: () => ({
      validd: false,
      dialog: false,
      dialogg: false,
      search: '',
      nameRules: [
        v => !!v || 'Le champ nom est obligatoire.',
      ],
      prenomRules: [
        v => !!v || 'Le champ prénom est obligatoire.',
      ],
      emailRules: [
        v => !!v || 'Le champ e-mail est obligatoire',
        v => /.+@.+/.test(v) || 'Veuillez introduire une adresse e-mail valide.',
      ],
      groupeRules: [
        v => !!v || 'Ce champ est obligatoire.',
      ],
      headers: [
        {
          text: 'Nom',
          align: 'left',
          sortable: true,
          value: 'Nom',
        },
        { text: 'Prénom', value: 'Prénom' },
        { text: 'Groupe', value: 'Groupe' },
        { text: 'Email', value: 'Email' },
        { text: 'Date de naissance', value: 'Date_de_naissance' },
        { text: 'Adresse', value: 'Adresse' },
        { text: 'Matricule', value: 'Matricule' },
        { text: 'Actions', value: 'action', sortable: false },
      ],
      Etudiants: [],
      editedIndex: -1,
      editedItem: {
        Nom: '',
        Prénom: '',
        Groupe:'',
        Email: '',
        Date_de_naissance: '',
        Adresse: '',
        Matricule:'',
      },
      defaultItem: {
        Nom: '',
        Prénom: '',
        Groupe:'',
        Email: '',
        Date_de_naissance: '',
        Adresse: '',
        Matricule:'',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Ajouter étudiant' : 'Modifier étudiant'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.Etudiants = [
          {
            Nom: 'Ghaouat',
            Prénom: 'Maroua',
            Groupe:'5',
            Email: 'mh_maroua@esi.dz',
            Date_de_naissance: '99/99/2019',
            Adresse: '7amizz',
            Matricule:'1311515633'
          },
          {
            Nom: 'Mouri',
            Prénom: 'Sid Ali Samy',
            Groupe:'10',
            Email: 'hd_mouri@esi.dz',
            Date_de_naissance: '05/03/2000',
            Adresse: 'Reghaia',
            Matricule:'1311515633'
          },
          {
            Nom: 'Gouttel',
            Prénom: 'Zack',
            Groupe:'7',
            Email: 'hz_gouttel@esi.dz',
            Date_de_naissance: '99/99/2019',
            Adresse: 'Stefi',
            Matricule:'1311515633'
          },
          {
            Nom: 'Ghaouat',
            Prénom: 'Maroua',
            Groupe:'11',
            Email: 'mh_maroua@esi.dz',
            Date_de_naissance: '99/99/2019',
            Adresse: '7amizz',
            Matricule:'1311515633'
          },
          {
            Nom: 'Mouri',
            Prénom: 'Sid Ali Samy',
            Groupe:'777',
            Email: 'hd_mouri@esi.dz',
            Date_de_naissance: '05/03/2000',
            Adresse: 'Reghaia',
            Matricule:'1311515633'
          },
          {
            Nom: 'Gouttel',
            Prénom: 'Zack',
            Groupe:'8',
            Email: 'hz_gouttel@esi.dz',
            Date_de_naissance: '99/99/2019',
            Adresse: 'Stefi',
            Matricule:'1311515633'
          },
          {
            Nom: 'Ghaouat',
            Prénom: 'Maroua',
            Groupe:'1',
            Email: 'mh_maroua@esi.dz',
            Date_de_naissance: '99/99/2019',
            Adresse: '7amizz',
            Matricule:'1311515633'
          },
          {
            Nom: 'Mouri',
            Prénom: 'Sid Ali Samy',
            Groupe:'0',
            Email: 'hd_mouri@esi.dz',
            Date_de_naissance: '05/03/2000',
            Adresse: 'Reghaia',
            Matricule:'1311515633'
          },
          {
            Nom: 'Gouttel',
            Prénom: 'Zack',
            Groupe:'5',
            Email: 'hz_gouttel@esi.dz',
            Date_de_naissance: '99/99/2019',
            Adresse: 'Stefi',
            Matricule:'1311515633'
          },
        ]
      },

      editItem (item) {
        this.editedIndex = this.Etudiants.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.Etudiants.indexOf(item)
        confirm('Voulez vous vraiment supprimer cet élément?') && this.Etudiants.splice(index, 1)
      },

      showitem(item){
        this.editedIndex = this.Etudiants.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogg = true
      },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {

        if (this.editedIndex > -1) {
          Object.assign(this.Etudiants[this.editedIndex], this.editedItem)
        } else {
          this.Etudiants.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>