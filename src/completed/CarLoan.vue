<template>
    <div class="min-h-screen bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
      <div class="max-w-7xl mx-auto">
        <h1 class="text-3xl font-bold text-center mb-8">Car Loan Amortization Calculator</h1>
        
        <!-- Calculator Form -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
          <div class="bg-white rounded-lg shadow p-6">
            <h2 class="text-xl font-semibold mb-4">Loan Details</h2>
            
            <div class="space-y-4">
              <div>
                <label for="carPrice" class="block text-sm font-medium text-gray-700">Car Price ($)</label>
                <input
                  id="carPrice"
                  v-model.number="carPrice"
                  type="number"
                  min="0"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
              
              <div>
                <label for="downPayment" class="block text-sm font-medium text-gray-700">Down Payment ($)</label>
                <input
                  id="downPayment"
                  v-model.number="downPayment"
                  type="number"
                  min="0"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
              
              <div>
                <label for="interestRate" class="block text-sm font-medium text-gray-700">Interest Rate (%)</label>
                <input
                  id="interestRate"
                  v-model.number="interestRate"
                  type="number"
                  min="0"
                  step="0.01"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
              
              <div>
                <label for="loanTerm" class="block text-sm font-medium text-gray-700">Loan Term (months)</label>
                <input
                  id="loanTerm"
                  v-model.number="loanTerm"
                  type="number"
                  min="1"
                  max="120"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
            </div>
            
            <div class="mt-6 bg-gray-50 p-4 rounded-md">
              <h3 class="text-lg font-medium">Loan Summary</h3>
              <div class="mt-2 grid grid-cols-2 gap-4">
                <div>
                  <p class="text-sm font-medium text-gray-500">Loan Amount</p>
                  <p class="text-sm">${{ formatCurrency(loanAmount) }}</p>
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-500">Monthly Payment</p>
                  <p class="text-sm">${{ formatCurrency(monthlyPayment) }}</p>
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-500">Total Interest</p>
                  <p class="text-sm">${{ formatCurrency(totalInterest) }}</p>
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-500">Total Cost</p>
                  <p class="text-sm">${{ formatCurrency(totalCost) }}</p>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Charts -->
          <div class="lg:col-span-2 space-y-8">
            <div class="bg-white rounded-lg shadow p-6">
              <h2 class="text-xl font-semibold mb-4">Principal vs Interest</h2>
              <div class="h-80">
                <canvas ref="pieChartRef"></canvas>
              </div>
            </div>
            
            <div class="bg-white rounded-lg shadow p-6">
              <h2 class="text-xl font-semibold mb-4">Amortization Schedule</h2>
              <div class="h-80">
                <canvas ref="lineChartRef"></canvas>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Amortization Table -->
        <div class="mt-12 bg-white rounded-lg shadow">
          <h2 class="text-xl font-semibold p-6">Amortization Schedule</h2>
          <div class="overflow-x-auto">
            <table class="min-w-full">
              <thead class="bg-gray-50">
                <tr>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Payment #</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Payment</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Principal</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Interest</th>
                  <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Remaining Balance</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-200">
                <tr v-for="(payment, index) in amortizationSchedule.slice(0, displayedRows)" :key="index">
                  <td class="px-6 py-4 text-sm text-gray-500">{{ payment.paymentNumber }}</td>
                  <td class="px-6 py-4 text-sm">${{ formatCurrency(payment.monthlyPayment) }}</td>
                  <td class="px-6 py-4 text-sm">${{ formatCurrency(payment.principal) }}</td>
                  <td class="px-6 py-4 text-sm">${{ formatCurrency(payment.interest) }}</td>
                  <td class="px-6 py-4 text-sm">${{ formatCurrency(payment.remainingBalance) }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="p-4 bg-gray-50 border-t">
            <button 
              v-if="displayedRows < amortizationSchedule.length" 
              @click="displayedRows += 12"
              class="text-sm font-medium text-indigo-600 hover:text-indigo-500"
            >
              Show more payments
            </button>
            <button 
              v-else-if="displayedRows > 12" 
              @click="displayedRows = 12"
              class="text-sm font-medium text-indigo-600 hover:text-indigo-500"
            >
              Show fewer payments
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, watch, onMounted } from 'vue';
  import { Chart } from 'chart.js/auto';
  
  // Form inputs with default values
  const carPrice = ref(30000);
  const downPayment = ref(5000);
  const interestRate = ref(4.5);
  const loanTerm = ref(60);
  const displayedRows = ref(12);
  
  // Chart references
  const pieChartRef = ref(null);
  const lineChartRef = ref(null);
  let pieChart = null;
  let lineChart = null;
  
  // Computed properties
  const loanAmount = computed(() => {
    return Math.max(0, carPrice.value - downPayment.value);
  });
  
  const monthlyPayment = computed(() => {
    if (loanAmount.value <= 0 || loanTerm.value <= 0 || interestRate.value <= 0) {
      return 0;
    }
    
    const monthlyRate = interestRate.value / 100 / 12;
    
    // Handle case where interest rate is very close to zero
    if (monthlyRate < 0.0001) {
      return loanAmount.value / loanTerm.value;
    }
    
    return loanAmount.value * monthlyRate * Math.pow(1 + monthlyRate, loanTerm.value) / 
           (Math.pow(1 + monthlyRate, loanTerm.value) - 1);
  });
  
  const totalCost = computed(() => {
    return monthlyPayment.value * loanTerm.value;
  });
  
  const totalInterest = computed(() => {
    return Math.max(0, totalCost.value - loanAmount.value);
  });
  
  // Calculate amortization schedule
  const amortizationSchedule = computed(() => {
    const schedule = [];
    
    if (loanAmount.value <= 0 || loanTerm.value <= 0 || interestRate.value <= 0) {
      return schedule;
    }
    
    const monthlyRate = interestRate.value / 100 / 12;
    let balance = loanAmount.value;
    let totalInterestPaid = 0;
    
    for (let i = 1; i <= loanTerm.value; i++) {
      const interestPayment = balance * monthlyRate;
      const principalPayment = monthlyPayment.value - interestPayment;
      balance -= principalPayment;
      totalInterestPaid += interestPayment;
      
      schedule.push({
        paymentNumber: i,
        monthlyPayment: monthlyPayment.value,
        principal: principalPayment,
        interest: interestPayment,
        totalInterestPaid: totalInterestPaid,
        remainingBalance: Math.max(0, balance)
      });
    }
    
    return schedule;
  });
  
  // Helper function to format currency
  const formatCurrency = (value) => {
    // Check if value is a valid number
    if (value === null || value === undefined || isNaN(value)) {
      return '0.00';
    }
    // Ensure value is treated as a number
    const numValue = Number(value);
    return numValue.toFixed(2).replace(/\d(?=(\d{3})+\.)/g, '$&,');
  };
  
  // Initialize and update charts
  const updateCharts = () => {
    if (pieChart) {
      pieChart.destroy();
    }
    
    if (lineChart) {
      lineChart.destroy();
    }
    
    // Create pie chart for principal vs interest
    if (pieChartRef.value) {
      pieChart = new Chart(pieChartRef.value, {
        type: 'pie',
        data: {
          labels: ['Principal', 'Interest'],
          datasets: [{
            data: [loanAmount.value, totalInterest.value],
            backgroundColor: ['#4F46E5', '#EC4899'],
            hoverOffset: 4
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              position: 'bottom'
            },
            tooltip: {
              callbacks: {
                label: function(context) {
                  const value = context.raw;
                  const total = context.dataset.data.reduce((a, b) => a + b, 0);
                  const percentage = Math.round((value / total) * 100);
                  return `${context.label}: $${formatCurrency(value)} (${percentage}%)`;
                }
              }
            }
          }
        }
      });
    }
    
    // Create line chart for amortization schedule
    if (lineChartRef.value && amortizationSchedule.value.length > 0) {
      // Prepare data for the line chart
      const labels = [];
      const principalData = [];
      const interestData = [];
      const balanceData = [];
      
      // Sample data points to avoid overcrowding the chart
      const step = Math.max(1, Math.floor(amortizationSchedule.value.length / 12));
      
      for (let i = 0; i < amortizationSchedule.value.length; i += step) {
        const payment = amortizationSchedule.value[i];
        labels.push(`Month ${payment.paymentNumber}`);
        
        // Cumulative principal paid
        const principalPaid = loanAmount.value - payment.remainingBalance;
        principalData.push(principalPaid);
        
        // Cumulative interest paid
        interestData.push(payment.totalInterestPaid);
        
        // Remaining balance
        balanceData.push(payment.remainingBalance);
      }
      
      // Add the final payment if not already included
      const lastPayment = amortizationSchedule.value[amortizationSchedule.value.length - 1];
      if (lastPayment && !labels.includes(`Month ${lastPayment.paymentNumber}`)) {
        labels.push(`Month ${lastPayment.paymentNumber}`);
        principalData.push(loanAmount.value);
        interestData.push(lastPayment.totalInterestPaid);
        balanceData.push(0);
      }
      
      lineChart = new Chart(lineChartRef.value, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [
            {
              label: 'Principal Paid',
              data: principalData,
              borderColor: '#4F46E5',
              backgroundColor: 'rgba(79, 70, 229, 0.1)',
              fill: true,
              tension: 0.1
            },
            {
              label: 'Interest Paid',
              data: interestData,
              borderColor: '#EC4899',
              backgroundColor: 'rgba(236, 72, 153, 0.1)',
              fill: true,
              tension: 0.1
            },
            {
              label: 'Remaining Balance',
              data: balanceData,
              borderColor: '#10B981',
              backgroundColor: 'rgba(16, 185, 129, 0.1)',
              fill: false,
              tension: 0.1
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
              ticks: {
                callback: function(value) {
                  return '$' + formatCurrency(value);
                }
              }
            }
          },
          plugins: {
            tooltip: {
              callbacks: {
                label: function(context) {
                  return `${context.dataset.label}: $${formatCurrency(context.raw)}`;
                }
              }
            }
          }
        }
      });
    }
  };
  
  // Watch for changes to update the charts
  watch(
    [carPrice, downPayment, interestRate, loanTerm],
    () => {
      updateCharts();
    },
    { deep: true }
  );
  
  // Initialize charts on component mount
  onMounted(() => {
    updateCharts();
  });
  </script>
  
  <style>
  input[type="number"] {
    -moz-appearance: textfield;
  }
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  </style>