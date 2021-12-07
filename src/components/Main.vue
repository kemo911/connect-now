<template>
  <b-container fluid>
    <b-row class="header">
      <b-col class="col-12 col-xl-3 d-flex xl-justify-content-end mb-3 mb-xl-0">
        <h2 class="header-video" :class="{ active: !showContact }" @click="showContact = !showContact">Video Games</h2>
      </b-col>
      <b-col class="col-12 col-xl-9">
        <h2 class="header-contact" :class="{ active: showContact }" @click="showContact = !showContact">Contact</h2>
      </b-col>
    </b-row>
    <b-row v-if="!showContact">
      <b-col class="col-12 col-xl-3 px-0">
        <Panel :state="state" @updateState="updateState" @clearData="clearData"/>
      </b-col>
      <b-col class="col-12 col-xl-9 px-0 pl-xl-4">
        <Body :games="games" />
      </b-col>
    </b-row>
	<b-row v-if="showContact" class="col-12 d-flex justify-content-center m-0">
		<ContactForm />
	</b-row>
  </b-container>
</template>
<script>
import Panel from "@/components/Sections/Panel";
import Body from "@/components/Sections/Body";
import ContactForm from '@/components/Sections/Contact.vue';

export default {
  name: "Main",
  components: { Body, Panel, ContactForm },
  data() {
    return {
		games: [],
		initialData: [],
		showContact: false,
		state: {
			name: "",
			minRating: 1,
			orderDirection: "down",
			orderingBy: "first_release_date",
		},
    };
  },
  mounted() {
    this.getGames();
  },
  methods: {
    async getGames() {
      try {
        const response = await this.$http.get(
          "https://public.connectnow.org.uk/applicant-test/"
        );
        this.games = this.handleData(response.data, this.state);
        this.initialData = response.data;
      } catch (error) {
        console.log(error);
      }
    },
    updateState([key, value]) {
      if (this.state[key]) {
        this.state[key] = value;
      }
    },
	normalizeText(text){
		return text.toLowerCase().normalize("NFD").replace(/\p{Diacritic}/gu, "")
	},
    handleData(data, state) {
		const { name, minRating, orderDirection, orderingBy } = state;
		const updatedData = data
			.map((item) => ({ ...item }))
			.filter((item) => {
				if (name !== "") {
					const searchTerms = name.split(" ");
					let includeAllTerms = true;
					const itemName = this.normalizeText(item.name);
					searchTerms.forEach((term) => {
					if (includeAllTerms) {
						const currentTerm = this.normalizeText(term);
						includeAllTerms = itemName.includes(currentTerm);
					}
					});
					return includeAllTerms;
				}
				return true;
			})
			.filter((item) => {
				return Math.ceil(item.rating / 10) >= minRating;
			})
			.sort((a, b) => {
				switch (orderingBy) {
					case "first_release_date":
						return orderDirection === "down"
							? new Date(a.first_release_date) -
								new Date(b.first_release_date)
							: new Date(b.first_release_date) -
								new Date(a.first_release_date);
					case "rating":
						return orderDirection === "down"
							? a.rating - b.rating
							: b.rating - a.rating;
					case "name":
						if(orderDirection === "down"){
							if(a.name < b.name) { return -1; }
							if(a.name > b.name) { return 1; }
							return 0;
						} else {
							if(a.name > b.name) { return -1; }
							if(a.name < b.name) { return 1; }
							return 0;
						}
					default: break;
				}
			});
		return updatedData
    },
	clearData(){
		this.state = {
			name: "",
			minRating: 1,
			orderDirection: "down",
			orderingBy: "first_release_date"
		}
	}
  },
  watch: {
    state: {
		handler: function (updatedState) {
			this.games = this.handleData(this.initialData, updatedState);
		},
		deep: true,
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container-fluid {
  max-width: 1600px;
  width: 90vw;
}
.header {
  padding: 4rem 0;
}

@media screen and (max-width: 1100px) {
  .header {
    padding-left: 2rem;
  }
}

.header h2 {
	text-transform: uppercase;
	position: relative;
	z-index: 1;
	cursor: pointer;
}

.header h2:hover {
	color: #c1d1e8; 
}

.header h2::before {
	position: absolute;
	z-index: -1;
	text-transform: uppercase;
	font-size: 4rem;
	opacity: 0.6;
	color: #182c47;
}


.header-video.active::before {
  content: "VIDEO";
  top: -2.6rem;
  left: -2rem;
}

.header-contact.active::before {
  content: "CONTACT";
  top: -2.6rem;
  left: 4rem;
}

.header-contact {
  padding-left: 6rem;
}
@media screen and (max-width: 1199px) {
  .header-contact {
    padding-left: 0rem;
  }
}
</style>