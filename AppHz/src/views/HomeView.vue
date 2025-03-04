<script>
import sims_uv_ce from '@/assets/data/tab_uv_15_rw_stats_ce.json'
import sims_re_ce from '@/assets/data/tab_re_15_rw_stats_ce.json'
import sims_uv_ys from '@/assets/data/tab_uv_15_rw_stats_ys.json'
import sims_re_ys from '@/assets/data/tab_re_15_rw_stats_ys.json'
import StatsBars from '@/components/StatsBars.vue'


export default {
  components: { StatsBars },
  data() {
    const gbp = new Intl.NumberFormat('en-GB', {
      style: 'currency',
      currency: 'GBP',
    });

    return {
      sims_uv_ys: sims_uv_ys,
      sims_re_ys: sims_re_ys,
      sims_uv_ce: sims_uv_ce,
      sims_re_ce: sims_re_ce,
      age0: 50,
      two_dose: true,
      sel_ys: undefined,
      sel_ce: undefined,
      stats: undefined,
      stats_def: [
        {Key: "Year_Life", Desc: "Life expectancy", direction: "+", upper: 40, fmt: (d) => d.toFixed(2)},
        {Key: "Q_Life_d", Desc: "Quality-adjusted life expectancy", direction: "+", upper: 40, fmt: (d) => d.toFixed(2)},
        {Key: "Year_Immunised", Desc: "Expected life year with vaccine-induced immunity", direction: "+", upper: 40, fmt: (d) => d.toFixed(2)},
        {Key: "Risk_HZ", Desc: "Life-time risk of HZ", direction: "+", upper: 0.3, fmt: (d) => (d * 100).toFixed(2) + "%"},
        {Key: "Risk_PHN", Desc: "Life-time risk of PHN", direction: "+", upper: 0.1, fmt: (d) => (d * 100).toFixed(2) + "%"},
        {Key: "C_Hosp_d", Desc: "Medical cost, hospitalisation", direction: "+", upper: 30, fmt: (d) => gbp.format(d.toFixed(2))},
        {Key: "C_GP_d", Desc: "Medical cost, primary care", direction: "+", upper: 30, fmt: (d) => gbp.format(d.toFixed(2))},
        {Key: "Q_HZ_d", Desc: "QALY loss due to HZ", direction: "-", upper: -0.1, fmt: (d) => d.toFixed(2)},
      ]
    }
  },
  methods: {
    select_scenario(age0, two_dose) {
      console.log(age0, two_dose);
      const sel = this.sims_uv_ys.filter((d) => d.Age0 >= age0);

      this.sel_ys = {
        'NoVac': sel.filter(d => d.Arm === "SOC")[0],
        'Vac': sel.filter(d => d.Arm === ((two_dose)?"RZV_2d":"RZV_1d"))[0],
      };

      this.sel_ce =  this.sims_uv_ce
        .filter((d) => d.Age0 >= age0)
        .filter(d => d.Arm === ((two_dose)?"RZV_2d":"RZV_1d"))[0];

      this.stats = this.stats_def.map(d => {
        const k = d.Key;
        return {
          Key: k,
          Desc: d.Desc,
          Direction: d.direction,
          Y0: this.sel_ys.NoVac[k],
          Y1: this.sel_ys.Vac[k],
          dY: this.sel_ce["d" + k],
          upper: d.upper,
          fmt: d.fmt
        }
      });
    },
  },
  watch: {
    age0(age_new) {
      if (age_new <= 50) age_new = 50;
      if (age_new >= 95) age_new = 95;
      this.select_scenario(age_new, this.two_dose);
    },
    two_dose(td_new) {
      this.select_scenario(this.age0, td_new);
    }
  },
  mounted() {
    this.select_scenario(this.age0, this.two_dose)
  },
}
</script>

<template>
  <div class="main">
    <h4>I'm</h4>
    <ul>
      <li>{{age0}} years old</li>
      <li v-if="two_dose"> With two doses vaccine</li>
      <li v-else>With single dose vaccine</li>
    </ul>

    <form>
      <div class="form-group">
        <label for="formControlInput1">My age </label>
        <input type="range" min="50" max="95" step="1" v-model="age0" />
      </div>
      <div class="form-group">
        <label for="formControlInput1">Two dose </label>
        <input type="checkbox" v-model="two_dose" />
      </div>
    </form>
    <stats-bars :stats="stats"></stats-bars>
  </div>
</template>

<style></style>
