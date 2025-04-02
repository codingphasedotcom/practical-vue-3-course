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
                  type="number"
                  min="0"
                  value="30000"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
              
              <div>
                <label for="downPayment" class="block text-sm font-medium text-gray-700">Down Payment ($)</label>
                <input
                  id="downPayment"
                  type="number"
                  min="0"
                  value="5000"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
              
              <div>
                <label for="interestRate" class="block text-sm font-medium text-gray-700">Interest Rate (%)</label>
                <input
                  id="interestRate"
                  type="number"
                  min="0"
                  step="0.01"
                  value="4.5"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
              
              <div>
                <label for="loanTerm" class="block text-sm font-medium text-gray-700">Loan Term (months)</label>
                <input
                  id="loanTerm"
                  type="number"
                  min="1"
                  max="120"
                  value="60"
                  class="mt-1 block w-full rounded-md border-gray-300 shadow-sm"
                />
              </div>
            </div>
            
            <div class="mt-6 bg-gray-50 p-4 rounded-md">
              <h3 class="text-lg font-medium">Loan Summary</h3>
              <div class="mt-2 grid grid-cols-2 gap-4">
                <div>
                  <p class="text-sm font-medium text-gray-500">Loan Amount</p>
                  <p class="text-sm">$25,000.00</p>
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-500">Monthly Payment</p>
                  <p class="text-sm">$466.81</p>
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-500">Total Interest</p>
                  <p class="text-sm">$3,008.60</p>
                </div>
                <div>
                  <p class="text-sm font-medium text-gray-500">Total Cost</p>
                  <p class="text-sm">$28,008.60</p>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Charts -->
          <div class="lg:col-span-2 space-y-8">
            <div class="bg-white rounded-lg shadow p-6">
              <h2 class="text-xl font-semibold mb-4">Principal vs Interest</h2>
              <div class="h-80">
                <!-- Chart placeholder -->
                <div class="w-full h-full flex items-center justify-center bg-gray-50 rounded-lg">
                  <p class="text-gray-500">Chart will be implemented here</p>
                </div>
              </div>
            </div>
            
            <div class="bg-white rounded-lg shadow p-6">
              <h2 class="text-xl font-semibold mb-4">Amortization Schedule</h2>
              <div class="h-80">
                <!-- Chart placeholder -->
                <div class="w-full h-full flex items-center justify-center bg-gray-50 rounded-lg">
                  <p class="text-gray-500">Chart will be implemented here</p>
                </div>
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
                <tr>
                  <td class="px-6 py-4 text-sm text-gray-500">1</td>
                  <td class="px-6 py-4 text-sm">$466.81</td>
                  <td class="px-6 py-4 text-sm">$373.56</td>
                  <td class="px-6 py-4 text-sm">$93.25</td>
                  <td class="px-6 py-4 text-sm">$24,626.44</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 text-sm text-gray-500">2</td>
                  <td class="px-6 py-4 text-sm">$466.81</td>
                  <td class="px-6 py-4 text-sm">$374.96</td>
                  <td class="px-6 py-4 text-sm">$91.85</td>
                  <td class="px-6 py-4 text-sm">$24,251.48</td>
                </tr>
                <tr>
                  <td class="px-6 py-4 text-sm text-gray-500">3</td>
                  <td class="px-6 py-4 text-sm">$466.81</td>
                  <td class="px-6 py-4 text-sm">$376.36</td>
                  <td class="px-6 py-4 text-sm">$90.45</td>
                  <td class="px-6 py-4 text-sm">$23,875.12</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="p-4 bg-gray-50 border-t">
            <button class="text-sm font-medium text-indigo-600 hover:text-indigo-500">
              Show more payments
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>

  <script setup>
  import {ref, computed, watch, onMounted} from 'vue';
  import { Chart } from 'chart.js/auto';
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