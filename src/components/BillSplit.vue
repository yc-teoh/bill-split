<template>
  <div>
    <h1>Bill Split</h1>
  </div>
  <div class="form">
    <div class="entry">
      <div class="particular-container">
        <div class="txt-label"><span>Your Amount:</span></div>
        <div><input v-model.number="bs.baseAmount" min=0 oninput="validity.valid||(value='');" type="number" class="txtbox" step=".01" placeholder="" /></div>
      </div>
    </div>
    <div class="entry">
      <div class="particular-container">
        <div class="txt-label"><span>Gross Total:</span></div>
        <div><input v-model.number="bs.grossTotal" min=0 oninput="validity.valid||(value='');" type="number" class="txtbox" step=".01" /></div>
      </div>
    </div>
    <div class="entry">
      <div class="particular-container">
        <div class="txt-label"><span>Sales Tax:</span></div>
        <div><input v-model.number="bs.salesTax" min=0 oninput="validity.valid||(value='');" type="number" class="txtbox" step=".01" /></div>
      </div>
    </div>
    <div class="entry">
      <div class="particular-container">
        <div class="txt-label"><span>Service Tax:</span></div>
        <div><input v-model.number="bs.serviceTax" min=0 oninput="validity.valid||(value='');" type="number" class="txtbox" step=".01" /></div>
      </div>
    </div>

    <div class="entry">
      <div class="txt-alert"><span>{{ errMsg.invalidValueErr || "" }}</span></div>
      <div class="particular-container">
        <div class="txt-label"><span>Amount Payable:</span></div>
        <div><span style="font-weight: bold">{{ result.total || "0.00" }}</span></div>
      </div>
    </div>

    <div>
      <button class="button" @click="clear">Clear</button>
      <button class="button" @click="calculate">Calculate</button>
    </div>
  </div>

  <div class="footer-txt">
    <span><a href="mailto:ycteoh@outlook.com">YC Teoh</a> © {{ new Date().getFullYear() }}</span>
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
      result: {},
      errMsg: {
        baseAmountErr: "",
        grossTotalErr: "",
        salesTaxErr: "",
        serviceTaxErr: "",
        invalidValueErr: ""
      }
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
      this.errMsg.baseAmountErr = "";
      this.errMsg.salesTaxErr = "";
      this.errMsg.serviceTaxErr = "";
      this.errMsg.invalidValueErr = "";
    },
    
    calculate() {
      let redFlag = 0;
      const initTotal = 0;
      this.result = {initTotal, amountPayable: initTotal};
      this.errMsg.baseAmountErr = "";
      this.errMsg.salesTaxErr = "";
      this.errMsg.serviceTaxErr = "";
      this.errMsg.invalidValueErr = "";

      const { baseAmountValid, grossTotalValid, salesTaxValid, serviceTaxValid } = this;

      if (!baseAmountValid || !grossTotalValid || !salesTaxValid || !serviceTaxValid) {
        return;
      }

      const { baseAmount, grossTotal, salesTax, serviceTax } = this.bs;

      if ((baseAmount < 0) || (grossTotal < 0) ||
        (salesTax < 0) || (serviceTax < 0) ||
        (baseAmount > grossTotal) || (salesTax > grossTotal) ||
        (serviceTax > grossTotal)) {
        this.errMsg.invalidValueErr = "Invalid value(s)!";
        redFlag++;
      }

      if (redFlag === 0) {
        let singlePct = 0;

        if (baseAmount === 0 && grossTotal === 0 && salesTax === 0 && serviceTax === 0) {
          singlePct = 0;
        } else {
          singlePct = (baseAmount / grossTotal);
        }

        const paySalesTax = salesTax * singlePct;
        const payServiceTax = serviceTax * singlePct;
        const actualTotal = baseAmount + paySalesTax + payServiceTax;
        const total = actualTotal.toFixed(2);
        console.log(total);
        this.result = {total, amountPayable: total};
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import "./style.css";
</style>
