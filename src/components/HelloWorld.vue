<template>
  <v-container fluid>
    <div v-show="start">
    <v-text-field label="Identifiant" v-model="identifiant"></v-text-field>
    <v-text-field label="Mot de passe" v-model="password"></v-text-field>
    <v-btn v-on:click="sInscrire">Inscription</v-btn>
    <v-btn v-on:click="seConnecter">Connexion</v-btn>
    </div>
    <div v-show="pendu">
    <p v-show="indice" class="text-center">{{words[n].indice}}</p>
    <v-row justify:space arround>
      <v-col v-for="l in words[n].longueur" v-bind:key="l">
        <v-card>
          <p class="text-center">{{numero_champs_texte[l-1]}}</p>
        </v-card>
      </v-col>
    </v-row>
    <v-text-field @keyup.enter="conditionValidation" v-bind:background-color="couleur" label="Entrez votre réponse" v-model="lettre"></v-text-field>
    <p>Entrez 1 seule charactère</p>
    <p> Lettres déjà utilisées : {{lettreEntree}}</p>
    <v-btn block rounded x-large v-on:click="conditionValidation" class="center">Valider</v-btn>
    </div>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    words: [
      {
        mot: 'nuage',
        longueur: 5,
        indice: 'Dans le ciel'
      },
      {
        mot: 'soleil',
        longueur: 6,
        indice: 'Très chaud'
      },
      {
        mot: 'ordinateur',
        longueur: 10,
        indice: "Utile à l'ESIEA"
      }
    ],
    n: 0,
    l: 0,
    e: 0,
    start: true,
    pendu: false,
    identifiant: '',
    password: '',
    faute: '',
    fauteQ: '',
    lettre: '',
    couleur: '',
    error: '',
    success: 0,
    indice: false,
    premiere_lettre: '',
    deuxieme_lettre: '',
    troisieme_lettre: '',
    quatrieme_lettre: '',
    cinquieme_lettre: '',
    sixieme_lettre: '',
    septieme_lettre: '',
    huitieme_lettre: '',
    neuvieme_lettre: '',
    dixieme_lettre: '',
    lettreEntree: '',
    numero_champs_texte: [
      '?',
      '?',
      '?',
      '?',
      '?',
      '?',
      '?',
      '?',
      '?',
      '?'
    ]
  }),
  methods: {
    mot_suivant () {
      if (this.n === this.words.length - 1) {
        this.n = 0
       }else {
        this.n = this.n + 1
      }
      console.log(this.numero_champs_texte)
      for (var i = 0; i < this.numero_champs_texte.length; i++) {
        this.numero_champs_texte[i] = ' '
      }
      console.log(this.numero_champs_texte)
      console.log(this.n)
    },

    conditionValidation () {
      this.lettreEntree = this.lettreEntree + this.lettre.toLowerCase() + ' '
      if (this.lettre.length !== 1) {
        console.log(this.numero_champs_texte[2])
        this.couleur = 'red'
      } else if (this.words[this.n].mot.indexOf(this.lettre.toLowerCase()) === -1) {
        this.couleur = ''
        this.faute++
        this.fauteQ++
        if (this.fauteQ === 3) {
          this.indice = true
        }
      } else {
        this.success++
        for (var i = 0; i < this.words[this.n].mot.length; i++) {
          if (this.words[this.n].mot[i] === this.lettre.toLowerCase()) {
            this.numero_champs_texte[i] = this.lettre.toLowerCase()
          }
        }
        if (this.success === this.words[this.n].mot.length) {
          this.success = 0
          this.fauteQ = 0
          this.indice = false
          for (var i = 0; i < this.words[this.n].mot.length; i++) {
            this.numero_champs_texte[i] = '?'
          }
          this.lettreEntree = ''
          this.n++
        }
      }
      this.lettre = ''
    },
    async sInscrire () {
      await this.axios.post('http://localhost:4000/api/addLog', {
        login: this.identifiant,
        password: this.password
      })
    },

    async seConnecter () {
      const rep = await this.axios.post('http://localhost:4000/api/login', {
        user: this.identifiant,
        password: this.password
      })
      if (rep.data.message === 'connected') {
        this.pendu = true
        this.start = false
      }
    }
  }
}
</script>
