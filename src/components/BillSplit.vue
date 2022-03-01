<template>
  <div class="form">
    <!-- <div>
      <h1>{{ msg }}</h1>
    </div> -->
    <div class="entry">
      <div><span>Amount:</span></div>
      <div><input v-model.number="bs.baseAmount" class="txtbox" /></div>
    </div>
    <div class="entry">
      <div><span>Gross Total:</span></div>
      <div><input v-model.number="bs.grossTotal" class="txtbox" /></div>
    </div>
    <div class="entry">
      <div><span>Sales Tax:</span></div>
      <div><input v-model.number="bs.salesTax" class="txtbox" /></div>
    </div>
    <div class="entry">
      <div><span>Service Tax:</span></div>
      <div><input v-model.number="bs.serviceTax" class="txtbox" /></div>
    </div>

    <div class="entry">
      <div><span>Amount Payable:</span></div>
      <div><span>{{ result.total || "0.00" }}</span></div>
    </div>

    <div>
      <button @click="calculate">Calculate</button>
      <button @click="clear">Clear</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BillSplit',
  
  data() {
    return {
      bs: {
        baseAmount: 0,
        grossTotal: 0,
        salesTax: 0,
        serviceTax: 0,
        amountPayable: 0
      },
      result: {}
    }
  },

  computed: {
    baseAmountValid() {
      return +this.bs.baseAmount >= 0;
    },
    grossTotalValid() {
      return +this.bs.grossTotal >= 0;
    },
    salesTaxValid() {
      return +this.bs.salesTax >= 0;
    },
    serviceTaxValid() {
      return +this.bs.serviceTax >= 0;
    },
    amountPayableValid() {
      return +this.bs.amountPayable >= 0;
    }
  },

  methods: {
    clear() {
      // this.current = "",
      const total = 0;
      this.result = {total, amountPayable: total};
      console.log(("CLEAR clicked"));
      this.bs.baseAmount = 0;
      this.bs.grossTotal = 0;
      this.bs.salesTax = 0;
      this.bs.serviceTax = 0;
    },
    
    calculate() {
      const { baseAmountValid, grossTotalValid, salesTaxValid, serviceTaxValid } = this;
      if (!baseAmountValid || !grossTotalValid || !salesTaxValid || !serviceTaxValid) {
        return;
      }

      const { baseAmount ,grossTotal, salesTax, serviceTax } = this.bs;
      const singlePct = (baseAmount / grossTotal);
      const paySalesTax = salesTax * singlePct;
      const payServiceTax = serviceTax * singlePct;
      const actualTotal = baseAmount + paySalesTax + payServiceTax;
      const total = actualTotal.toFixed(2);
      console.log(total);
      this.result = {total, amountPayable: total};
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form {
  display: flex;
  flex-direction: column;
  margin: 0 auto;
  background-color: beige;
  width: 640px;
}

.entry {
  display: flex;
  justify-content: center;
  flex-direction: row;
}
</style>
