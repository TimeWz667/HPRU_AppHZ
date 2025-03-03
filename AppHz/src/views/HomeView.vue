<script>
import sims_uv_ce from '@/assets/data/tab_uv_15_rw_stats_ce.json'
import sims_re_ce from '@/assets/data/tab_re_15_rw_stats_ce.json'
import sims_uv_ys from '@/assets/data/tab_uv_15_rw_stats_ys.json'
import sims_re_ys from '@/assets/data/tab_re_15_rw_stats_ys.json'

export default {
  data() {
    return {
      sims_uv_ys: sims_uv_ys,
      sims_re_ys: sims_re_ys,
      sims_uv_ce: sims_uv_ce,
      sims_re_ce: sims_re_ce,
      age0: 50,
      two_dose: true,
      sel_ys: undefined,
      sel_ce: undefined,
      stats: undefined
    }
  },
  methods: {
    select_scenario(age0, two_dose) {
      console.log(age0, two_dose);
      //let sel = this.sims_uv_ys.filter((d) => d.Age0 === age0);
      console.log(this.sims_uv_ys.filter((d) => d.Age0 >= age0).filter(d => d.Arm === "SOC")[0])
      this.sel_ys = {
        'NoVac': this.sims_uv_ys.filter((d) => d.Age0 >= age0).filter(d => d.Arm === "SOC")[0],
        'Vac': this.sims_uv_ys.filter((d) => d.Age0 >= age0).filter(d => d.Arm === ((two_dose)?"RZV_2d":"RZV_1d"))[0],
      };

      this.sel_ce =  this.sims_uv_ce
        .filter((d) => d.Age0 >= age0)
        .filter(d => d.Arm === ((two_dose)?"RZV_2d":"RZV_1d"))[0];

      // console.log(this.sel_ce);
      // console.log(this.sel_ys)
      this.stats = ["Year_Life", "Year_Immunised", "Risk_HZ", "Risk_PHN",
        "C_Med_d", "C_Hosp_d", "C_GP_d", "Q_HZ_d", "Q_Life_d"].map(k => {
        return {
          Key: k,
          Y0: this.sel_ys.NoVac[k],
          Y1: this.sel_ys.Vac[k],
          dY: this.sel_ce["d" + k]
        }
      })
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
        <label for="exampleFormControlInput1">My age </label>
        <input type="range" min="50" max="95" step="1" v-model="age0" />
      </div>
      <div class="form-group">
        <label for="exampleFormControlInput1">Two dose </label>
        <input type="checkbox" v-model="two_dose" />
      </div>
    </form>

    <h3>Risk of shingles</h3>
    <div v-for="st in stats">{{st}}</div>
<!--    <h4>Value</h4>
    {{ sel_ys }}
    <h4>Difference</h4>
    {{ sel_ce }}
        report items-->
  </div>
</template>

<style></style>
