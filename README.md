# vue-inputNumber
vue number input component with thousands separator, number validation, 2 way binding input, max & min value, and bootstrap 4 support


## Installation
download/clone and place it to your components directory

## Registering Component

  Locale 
  
     import inputNumber from '~/components/inputNumber.vue'
   
     components: {
       inputNumber
     }

  Global
  
     import inputNumber from '~/components/inputNumber.vue'
     
     Vue.component('inputNumber')
     
     
 ## Parameter

    v-model : 2 ways binding to your model variable (Number)
    min-value : minimum value (Number)
    max-value : maximum value (Number)
    class : styling class you want to add to the component" (String)
    disabled: set disabled state of component (Boolean)
  
  
 ## Events
 
    @input : event fired when user typing some value into component
    @foces: event fired when component got focussed
    
## Sample
    
    <inputNumber
      v-model="price"
      :min-value="0"
      :max-value="1000000"
      class="form-control"
      @input="handlePriceInput"
      disabled
    />
 
  
    
