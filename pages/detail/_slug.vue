<template>
  <div class="mx-28 my-10">
    <nuxt-link to="/"
      ><div>
        <button class="btn btn-md border-none hover:bg-[#673fe9] bg-[#8362F2]">
          <span class="pr-2"><img src="@/assets/img/Vector.png" alt="" /></span>
          Back to home page
        </button>
      </div></nuxt-link
    >
    <div class="flex flex-col mt-8 mb-4">
      <h1 class="text-4xl font-bold flex flex-row">
        {{ detailCountry.name }}
        <span class="pl-2 pt-1"
          ><img :src="detailCountry.flag" class="w-[58px]" alt=""
        /></span>
      </h1>
      <div class="flex flex-row space-x-2 mt-2">
        <div class="badge badge-sm border-none bg-[#8DD4CC]">
          {{ detailCountry.spelling.id }}
        </div>
        <div class="badge badge-sm border-none bg-[#8DD4CC]">
          {{ detailCountry.spelling.nameSpell }}
        </div>
        <div class="badge badge-sm border-none bg-[#8DD4CC]">
          {{ detailCountry.spelling.nameFullSpell }}
        </div>
      </div>
    </div>
    <div class="grid grid-cols-2 gap-4">
      <div class="bg-[#FFFFFF] shadow-md rounded pl-4 py-8">
        <h1 class="mb-2 font-bold">LatLong</h1>
        <div class="text-[#8362F2] text-4xl font-bold">
          {{ detailCountry.position.lat }},
          <span>{{ detailCountry.position.long }}</span>
        </div>
      </div>
      <div class="bg-[#FFFFFF] shadow-md rounded pl-4 py-8">
        <ul>
          <li>
            Capital: <span class="font-bold">{{ detailCountry.capital }}</span>
          </li>
          <li>
            Region: <span class="font-bold">{{ detailCountry.region }}</span>
          </li>
          <li>
            Subregion:
            <span class="font-bold">{{ detailCountry.subRegion }}</span>
          </li>
        </ul>
      </div>
      <div class="bg-[#FFFFFF] shadow-md rounded pl-4 py-6">
        <h1 class="mb-2 font-bold">Calling Code</h1>
        <div class="text-[#8362F2] text-4xl font-bold mb-1">
          {{ detailCountry.callingCode }}
        </div>
        <div>
          <p>
            <span
              class="underline cursor-pointer text-[#8362F2]"
              @mouseenter="isHovered = true"
              @mouseleave="isHovered = false"
              >{{ listCallingCode.length }} country</span
            >
            with this calling code
          </p>
          <div
            class="absolute bg-[#525252] rounded text-slate-100 px-3 py-2 mt-2"
            v-if="isHovered"
          >
            <div v-for="(item, key) in listCallingCode" :key="key">
              {{ item.name }}
            </div>
          </div>
        </div>
      </div>
      <div class="bg-[#FFFFFF] shadow-md rounded pl-4 py-6">
        <h1 class="mb-2 font-bold">Currency</h1>
        <div class="text-[#8362F2] text-4xl font-bold mb-1">
          {{ detailCountry.currency.code }}
        </div>
        <div>
          <p>
            <span
              class="underline cursor-pointer text-[#8362F2]"
              @mouseenter="isHovered2 = true"
              @mouseleave="isHovered2 = false"
              >{{ listCurrencies.length }} country</span
            >
            with this currency
          </p>
          <div
            class="absolute bg-[#525252] rounded text-slate-100 px-3 py-2 mt-2"
            v-if="isHovered2"
          >
            <div v-for="(item, key) in listCurrencies" :key="key">
              {{ item.name }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      isHovered: false,
      isHovered2: false,
      detailCountry: {
        name: null,
        position: {
          lat: null,
          long: null,
        },
        flag: null,
        spelling: {
          id: null,
          nameFullSpell: null,
          nameSpell: null,
        },
        callingCode: null,
        currency: {
          code: null,
        },
        capital: null,
        region: null,
        subRegion: null,
      },
      listCallingCode: [],
      listCurrencies: [],
    };
  },
  mounted() {
    this.fetchDetailCountry(this.$route.params.slug);
  },
  methods: {
    async fetchDetailCountry(val) {
      await fetch(`https://restcountries.com/v2/name/${val}?fullText=true`)
        .then((response) => response.json())
        .then((data) => {
          const detail = data.pop();
          this.detailCountry.name = detail.name;
          this.detailCountry.position.lat = detail.latlng[0];
          this.detailCountry.position.long = detail.latlng[1];
          this.detailCountry.flag = detail.flags.png;
          this.detailCountry.spelling.id = detail.altSpellings[0];
          this.detailCountry.spelling.nameSpell = detail.altSpellings[1];
          this.detailCountry.spelling.nameFullSpell = detail.altSpellings[2];
          this.detailCountry.callingCode = detail.callingCodes[0];
          this.detailCountry.currency.code = detail.currencies[0].code;
          this.detailCountry.capital = detail.capital;
          this.detailCountry.region = detail.region;
          this.detailCountry.subRegion = detail.subregion;

          this.fetchCallingCode(this.detailCountry.callingCode);
          this.fetchCurrencies(this.detailCountry.currency.code);
        });
    },
    async fetchCallingCode(val) {
      await fetch(`https://restcountries.com/v2/callingcode/${val}`)
        .then((response) => response.json())
        .then((data) => (this.listCallingCode = data));
    },
    async fetchCurrencies(val) {
      await fetch(`https://restcountries.com/v2/currency/${val}`)
        .then((response) => response.json())
        .then((data) => (this.listCurrencies = data));
    },
  },
};
</script>
