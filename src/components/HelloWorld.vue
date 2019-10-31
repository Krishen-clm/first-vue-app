<template>
  <v-container fluid>
    <div v-show="start">
    <v-text-field label="Identifiant" v-model="identifiant"></v-text-field>
    <v-text-field label="Mot de passe" v-model="password"></v-text-field>
    <v-btn v-on:click="sInscrire">Inscription</v-btn>
    <v-btn v-on:click="seConnecter">Connexion</v-btn>
    <v-alert v-show="useCreate" type="success">Compte bien créé</v-alert>
    <v-alert v-show="userExist" type="error">Ce nom d'utilisateur est déjà pris</v-alert>
    <v-alert v-show="userWrong" type="error">Nom d'utilisateur ou mot de passe incorrecte</v-alert>
    </div>
    <div v-show="pendu">
    <v-btn v-on:click="seDeconnecter">Déconnexion</v-btn>
    <p v-show="indice" class="text-center">{{words[n].indice}}</p>
    <v-row justify:space arround>
      <v-col v-for="l in words[n].longueur" v-bind:key="l">
        <v-card>
          <p class="text-center">{{numero_champs_texte[l-1]}}</p>
        </v-card>
      </v-col>
    </v-row>
    <v-text-field v-on:click="couleurReset" v-on:keyup.enter="conditionValidation" v-bind:background-color="couleur" label="Entrez votre réponse" v-model="lettre"></v-text-field>
    <p>Entrez 1 seul charactère</p>
    <p> Lettres déjà utilisées : {{lettreEntree}}</p>
    <v-btn block rounded x-large v-on:click="conditionValidation" class="center">Valider</v-btn>
    <v-img v-show="fauteQ != 0" v-bind:src="penduImage[this.fauteQ]" max-width="400"></v-img>
    </div>
    <div v-show="perdu">
      <v-btn v-on:click="seDeconnecter">Déconnexion</v-btn>
      <h1>VOUS AVEZ PERDU </h1>
      <v-btn v-on:click="reessayer">Essayer de nouveau</v-btn>
      <v-img v-bind:src="penduImage[12]" max-width="400"></v-img>
    </div>
    <div v-show="gagner">
      <h1>Vous Avez Gagné </h1>
      <v-btn v-on:click="seDeconnecter">Se déconnecter</v-btn>
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
      },
      {
        mot: 'anticonstitutionnellement',
        longueur: 25,
        indice: 'Choinx'
      },
      {
        mot: 'tablette',
        longueur: 8,
        indice: 'adoré par les darons'
      },
      {
        mot: 'conclusion',
        longueur: 10,
        indice: 'terminer'
      },
      {
        mot: 'mobilier',
        longueur: 8,
        indice: 'à la maison'
      }
    ],
    n: 0,
    l: 0,
    e: 0,
    start: true,
    pendu: false,
    gagner: false,
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
    useCreate: false,
    userExist: false,
    userWrong: false,
    perdu: false,
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
      '?',
      '?',
      '?',
      '?',
      '?',
      '?',
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
    ],
    penduImage: [
      require('@/assets/pendu1.jpg'),
      require('@/assets/pendu2.jpg'),
      require('@/assets/pendu3.jpg'),
      require('@/assets/pendu4.jpg'),
      require('@/assets/pendu5.jpg'),
      require('@/assets/pendu6.jpg'),
      require('@/assets/pendu7.jpg'),
      require('@/assets/pendu8.jpg'),
      require('@/assets/pendu9.jpg'),
      require('@/assets/pendu10.jpg'),
      require('@/assets/pendu11.jpg'),
      require('@/assets/pendu12.jpg'),
      require('@/assets/pendu13.jpg')
    ]
  }),
  methods: {
    mot_suivant () {
      if (this.n === this.words.length - 1) {
        this.n = 0
      } else {
        this.n = this.n + 1
      }
      for (var w = 0; w < this.numero_champs_texte.length; w++) {
        this.numero_champs_texte[w] = ' '
      }
    },

    conditionValidation () {
      if (this.lettreEntree.indexOf(this.lettre.toLowerCase()) === -1) {
        if (this.lettre.length !== 1) {
          this.couleur = 'red'
          return ''
        } else if (this.words[this.n].mot.indexOf(this.lettre.toLowerCase()) === -1) {
          this.lettreEntree = this.lettreEntree + this.lettre.toLowerCase() + ' '
          this.couleur = ''
          this.faute++
          this.fauteQ++
          if (this.fauteQ === 3) {
            this.indice = true
          }
          if (this.fauteQ === 12) {
            this.pendu = false
            this.perdu = true
            this.faute = 0
            this.fauteQ = 0
            this.n = 0
            this.lettreEntree = ''
          }
          if (this.n + 1 === this.words.length) {
            this.pendu = false
            this.gagner = true
            this.faute = 0
            this.fauteQ = 0
            this.n = 0
            this.lettreEntree = ''
          }
        } else {
          this.lettreEntree = this.lettreEntree + this.lettre.toLowerCase() + ' '
          const occurence = this.words[this.n].mot.split(this.lettre)
          this.success = this.success + occurence.length - 1
          for (var l = 0; l < this.words[this.n].mot.length; l++) {
            if (this.words[this.n].mot[l] === this.lettre.toLowerCase()) {
              this.numero_champs_texte[l] = this.lettre.toLowerCase()
            }
          }
          if (this.success === this.words[this.n].mot.length) {
            this.success = 0
            this.fauteQ = 0
            this.indice = false
            for (var m = 0; m < this.words[this.n].mot.length; m++) {
              this.numero_champs_texte[m] = '?'
            }
            this.lettreEntree = ''
            this.n++
          }
        }
        if (this.n + 1 === this.words.length) {
          this.pendu = false
          this.gagner = true
          this.faute = 0
          this.fauteQ = 0
          this.n = 0
          this.lettreEntree = ''
        }
        this.lettre = ''
        this.lettreEntree = this.lettreEntree + this.lettre.toLowerCase() + ' '
      }
    },
    async sInscrire () {
      const rep = await this.axios.post('/api/addLog', {
        login: this.identifiant,
        password: this.password
      })
      if (rep.data.message === 'user created succesfull') {
        this.userWrong = false
        this.userExist = false
        this.useCreate = true
      } else {
        this.useCreate = false
        this.userWrong = false
        this.userExist = true
      }
    },

    async seConnecter () {
      const rep = await this.axios.post('/api/login', {
        login: this.identifiant,
        password: this.password
      })
      if (rep.data.message === 'connected') {
        this.pendu = true
        this.start = false
      } else {
        this.userExist = false
        this.useCreate = false
        this.userWrong = true
      }
    },

    async seDeconnecter () {
      await this.axios.get('/api/logout', {
      })
      this.pendu = false
      this.success = 0
      this.fauteQ = 0
      this.indice = false
      this.perdu = false
      this.gagner = false
      this.start = true
      this.useCreate = false
      this.userWrong = false
      this.userExist = false
      this.lettreEntree = ''
    },
    reessayer () {
      this.perdu = false
      this.pendu = true
      this.indice = false
    },

    couleurReset () {
      this.couleur = ''
      this.lettre = ''
    }
  }
}
</script>
