<template>
  <div class="calculator">
    <h1>Mortgage Calculator</h1>
    <label for="">Home Cost ($)</label>
    <input type="number" placeholder="250,000" v-model="homeCostAmt" />
    <label for="">Down Payment Amount ($)</label>
    <input type="number" placeholder="10,000" v-model="downPaymentAmt" />
    <label for="">Yearly Interest Rate (%)</label>
    <input type="number" placeholder="4.2" v-model="yearlyInterestRate" />
    <label for="">Mortgage Duration (years)</label>
    <input type="number" placeholder="30" v-model="durationYears" />
    <label for=""
      ><span class="optional">Optional </span>Monthly Property Tax ($)</label
    >
    <input type="text" placeholder="215" v-model="monthlyPropertyTax" />
    <label for=""
      ><span class="optional">Optional </span>Monthly Homeowners Insurance
      ($)</label
    >
    <input type="text" placeholder="105" v-model="monthlyHomeOwnersInsurance" />
    <p v-if="estimatedPayment" class="estimated-payment">
      Your Estimated Payment is
      <transition name="fade" mode="out-in">
        <b :key="estimatedPayment">${{ estimatedPayment }}</b>
      </transition>
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      downPaymentAmt: "",
      homeCostAmt: "",
      yearlyInterestRate: "",
      durationYears: "",
      monthlyMortgagePayment: "",
      monthlyPropertyTax: "",
      monthlyHomeOwnersInsurance: "",
    };
  },
  computed: {
    principalAmt: function() {
      return this.homeCostAmt - this.downPaymentAmt;
    },
    monthlyInterestRate: function() {
      if (this.yearlyInterestRate == 0) {
        return 0;
      } else {
        return this.yearlyInterestRate / 100 / 12;
      }
    },
    durationMonths: function() {
      return this.durationYears * 12;
    },
    estimatedPayment: function() {
      let p = Number(this.principalAmt);
      let i = Number(this.monthlyInterestRate);
      let n = Number(this.durationMonths);
      let x = i * (1 + i) ** n;
      let y = (1 + i) ** n - 1;
      let z = p * (x / y);

      //If the interest rate is 0, the mortgage payment equation is simply (P / N)
      if (i === 0 && p && n) {
        if (this.monthlyPropertyTax && this.monthlyHomeOwnersInsurance) {
          let est =
            p / n +
            Number(this.monthlyPropertyTax) +
            Number(this.monthlyHomeOwnersInsurance);
          return est.toFixed(0);
        }
        if (this.monthlyPropertyTax > 0) {
          let est = p / n + Number(this.monthlyPropertyTax);
          return est.toFixed(0);
        }
        if (this.monthlyHomeOwnersInsurance > 0) {
          let est = p / n + Number(this.monthlyHomeOwnersInsurance);
          return est.toFixed(0);
        }
        let est = p / n;
        return est.toFixed(0);
      }
      //If the interest rate is non-zero:
      if (!isFinite(z)) {
        return null;
      }
      if (this.monthlyPropertyTax && this.monthlyHomeOwnersInsurance) {
        let est1 =
          Number(z) +
          Number(this.monthlyPropertyTax) +
          Number(this.monthlyHomeOwnersInsurance);
        return est1.toFixed(0);
      }
      if (this.monthlyHomeOwnersInsurance > 0) {
        let est2 = Number(z) + Number(this.monthlyHomeOwnersInsurance);
        return est2.toFixed(0);
      }
      if (this.monthlyPropertyTax > 0) {
        let est3 = Number(z) + Number(this.monthlyPropertyTax);

        return est3.toFixed(0);
      }
      return z.toFixed(0);
    },
  },
};
</script>

<style>
.calculator {
  width: 375px;
  margin: 0px auto;
  padding: 0px 10px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

label {
  margin: 0px 0px 4px 0px;
  display: block;
}
input {
  box-sizing: border-box;
  padding: 8px 8px;
  margin: 0px 0px 16px 0px;
  width: 100%;
  font-size: 16px;
}
input::placeholder {
  color: rgb(179, 179, 179);
}
.optional {
  font-size: 12px;
  background-color: rgb(43, 170, 138);
  color: white;
  padding: 2px;
  border-radius: 5px;
  margin-right: 5px;
}
.estimated-payment {
  font-size: 28px;
}

@media screen and (max-width: 400px) {
  h1 {
    font-size: 24px;
    width: 100%;
  }
  .calculator {
    width: 325px;
  }
  label {
    font-size: 14px;
  }
}
</style>
