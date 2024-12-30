<template>
  <div id="app">
    <div class="list list_container">
        
      <div class="header">
          <h1 id="title">{{ title }}</h1>
          
          <div class="bt_A btA_out">
              <button v-if="editing" @click="doEdit(false); UPnCB(true)" id="cancel" class="bt" >Cancel</button>
              <button v-else @click="doEdit(true)" id="add" class="bt" >Add</button>
          </div>            
      </div>
      
      <form 
        @submit.prevent="add_pd" 
        v-if="editing"
        class="add_product">
          <div class="inNbt_out">
            <div class="inNbt"
                  :class="showUPnCB ? 'original' : 'expand'">
              <section class="enter_out">
                <div class="enter_in">
                  <div class="input_c">
                    <input 
                      v-model.trim="newProduct" 
                      v-on:keyup.enter="add_pd, noblank, nosubmit" 
                      class="product_in"
                      type="text" 
                      placeholder="Product"
                      name="product"/>
                    <div id="x_icon_out" @click="clear">
                      <svg id="x_icon" width="24" height="24" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path id="x_X" d="M16.3394 9.32245C16.7434 8.94589 16.7657 8.31312 16.3891 7.90911C16.0126 7.50509 15.3798 7.48283 14.9758 7.85938L12.0497 10.5866L9.32245 7.66048C8.94589 7.25647 8.31312 7.23421 7.90911 7.61076C7.50509 7.98731 7.48283 8.62008 7.85938 9.0241L10.5866 11.9502L7.66048 14.6775C7.25647 15.054 7.23421 15.6868 7.61076 16.0908C7.98731 16.4948 8.62008 16.5171 9.0241 16.1405L11.9502 13.4133L14.6775 16.3394C15.054 16.7434 15.6868 16.7657 16.0908 16.3891C16.4948 16.0126 16.5171 15.3798 16.1405 14.9758L13.4133 12.0497L16.3394 9.32245Z"/><path id="x_O" fill-rule="evenodd" clip-rule="evenodd" d="M1 12C1 5.92487 5.92487 1 12 1C18.0751 1 23 5.92487 23 12C23 18.0751 18.0751 23 12 23C5.92487 23 1 18.0751 1 12ZM12 21C7.02944 21 3 16.9706 3 12C3 7.02944 7.02944 3 12 3C16.9706 3 21 7.02944 21 12C21 16.9706 16.9706 21 12 21Z"  /></svg>                   
                    </div>
                  </div>
                      <!-- High Priority -->
                      <label class="l_HP">
                          <input type="checkbox" class="HP" v-model="newProductHP"> 
                            High Priority
                      </label>

                      <!--counter 數字數 -->
                      <p class="counter" :class="{overlimit: characterCount>20}"> 
                        Character Count：{{characterCount}}/20 
                      </p>

                </div>
              </section>
              
                  
                  
                  <!-- save -->
                  <div class="btB_out save_out" > 
                        <button 
                          v-bind:disabled="newProduct.length === 0"
                          class="bt"
                          @click="nosubmit" 
                          id="save"
                          >Save</button>     
                  </div>   

                  <div class="delNup btB_out">
                  <!-- delete -->
                    <div 
                      v-if="showUPnCB"
                      @click="UPnCB(false)">
                      <button 
                        :disabled="!products.length"
                        class="bt"
                        id="del"
                        >Delete</button>
                    </div>
                      
                      <!-- up -->                        
                      <div v-else id="upNot">
                        <div id="up_out">
                          <button
                            @click="UPnCB(true)"        
                            class="bt"
                            id="up_icon_bt">
                              <svg id="up_icon" width="27" height="27" xmlns="http://www.w3.org/2000/svg"  viewBox="0 0 448 512">><path d="M201.4 137.4c12.5-12.5 32.8-12.5 45.3 0l160 160c12.5 12.5 12.5 32.8 0 45.3s-32.8 12.5-45.3 0L224 205.3 86.6 342.6c-12.5 12.5-32.8 12.5-45.3 0s-12.5-32.8 0-45.3l160-160z"/></svg>
                          </button>
                        </div>
                          
                        <div class="others">
                          <!-- all -->
                          <label id="del_all">
                            <input type="checkbox" 
                              v-model="ALLcheckbox" 
                              @click="allCheckedProducts"
                              >ALL</input>
                          </label>

                          <!-- 刪除框勾選數量 -->
                          <p id="selected">Selected：{{ selectedCount }}/{{ products.length }}</p>

                          
                          <!-- refresh_icon -->
                          <div id="refresh_icon_out" 
                              @click="clearCheckedProducts"
                              :class="{emptySC: selectedZero}">
                            <svg id="refresh_icon" width="27" height="30" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M463.5 224l8.5 0c13.3 0 24-10.7 24-24l0-128c0-9.7-5.8-18.5-14.8-22.2s-19.3-1.7-26.2 5.2L413.4 96.6c-87.6-86.5-228.7-86.2-315.8 1c-87.5 87.5-87.5 229.3 0 316.8s229.3 87.5 316.8 0c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0c-62.5 62.5-163.8 62.5-226.3 0s-62.5-163.8 0-226.3c62.2-62.2 162.7-62.5 225.3-1L327 183c-6.9 6.9-8.9 17.2-5.2 26.2s12.5 14.8 22.2 14.8l119.5 0z"/></svg>      
                          </div> 

                          <!-- trash icon --> 
                          <div id="trash_icon_out" 
                              @click="delProducts"
                              :class="{emptySC: selectedZero}">
                            <svg id="trash_icon" width="25" height="32"  xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><path d="M135.2 17.7L128 32 32 32C14.3 32 0 46.3 0 64S14.3 96 32 96l384 0c17.7 0 32-14.3 32-32s-14.3-32-32-32l-96 0-7.2-14.3C307.4 6.8 296.3 0 284.2 0L163.8 0c-12.1 0-23.2 6.8-28.6 17.7zM416 128L32 128 53.2 467c1.6 25.3 22.6 45 47.9 45l245.8 0c25.3 0 46.3-19.7 47.9-45L416 128z"/></svg>
                          </div>

                        </div>

                      </div>
                  </div>
            </div>
          </div>
          
      </form>

      <section class="scrollable">
        <ul class="pd_list">
          <li 
              v-for="(product, index) in reverseProducts" 
              
              :key="product.id" 
              :class="{strikeout: product.purchased,
                      priority: product.highPriority}">
              <template v-if="showUPnCB">
                  <span @click="togglePurchased(product)">
                      {{ product.object }}                         
                  </span>   
              </template>
              
              <template v-else>
                  <label>
                      <input type="checkbox" 
                              @click.stop
                              v-model="product.checked"
                              >
                          {{ product.object }}
                      </input>
                  </label>
              </template>
                  
          </li>
        </ul>
        <p id="none_pd" v-if="!products.length">
            Nothing to see here
        </p>
      </section>
            
    </div>
  </div>
</template>

<script scoped>
export default {
  data() {
    return {
      title: "Shopping List", // 標題文字 
    };
  },
};
</script>

<script setup>
import { ref, computed, watch} from "vue";

const products = ref(
            [
                {id: 1, object:'6 pens', purchased: true, highPriority: false, checked: false},                             
                {id: 2, object:'5 bagels', purchased: false, highPriority: false, checked: false},             
                {id: 3, object:'13 mangos', purchased: true, highPriority: true, checked: false},             
                {id: 4, object:'1 sugar', purchased: false, highPriority: true, checked: false},      
                {id: 5, object:'3 face masks', purchased: false, highPriority: false, checked: false}
            ]
        );

const newProduct = ref("");

const newProductHP = ref(false);

const add_pd = ()=>
  {products.value.push(
    {id: products.value.length+1, 
    object: newProduct.value,
    highPriority: newProductHP.value
  })
  newProduct.value = ""  // 送出後同時清除框
  newProductHP.value = "" // 送出後同時清除勾
  };

const editing = ref(false);

const doEdit = (e)=>{
  editing.value = e
  newProduct.value = ""
  newProductHP.value = ""
};

const togglePurchased = (products) =>{
  products.purchased = !products.purchased
};

const characterCount = computed(()=>{
  return newProduct.value.length
});


const nosubmit = (e) => {
  if(newProduct.value.trim().length > 20) 
  {e.preventDefault();
    alert("輸入框不能超過20個字！");           
  }
};


const clear = () => {
  newProduct.value = "";
};        

const noblank = () => {
  if (newProduct.value.trim().length === 0) {
    newProduct.value = ""; // 清除多餘空白
  }
};       

const reverseProducts = computed (()=>{
    return [...products.value].reverse()
});

const showUPnCB = ref(true);   
//初始狀態下showUPnCB是開啟的 先展示Delete
//如果是false togglePurchased應先暫停

const UPnCB = (uc) => {
  showUPnCB.value = uc;
  clearCheckedProducts();
};  

const allCheckedProducts = () => {
  if (this.ALLcheckbox) {
      products.value.forEach(product => {
          product.checked = true;
      });
  } else {
      products.value.forEach(product => {
          product.checked = false;
      }); 
  }
};

const removeCheckedProducts = () => {
  products.value = products.value.filter(product => !product.checked);
  ALLcheckbox.value = false;
};

const clearCheckedProducts = () => {
  products.value.forEach(product => {
    product.checked = false;
  });   
  ALLcheckbox.value = false;
};

const delProducts = () => {
  const confirmed  = window.confirm("確定要刪除嗎?");
  if (confirmed) {
    removeCheckedProducts();
  } else {
    clearCheckedProducts();
  }
};

const ALLcheckbox = ref(false); 

const selectedCount = computed(() => 
  products.value.filter(product => product.checked).length
);

const selectedZero = computed(() => 
  products.value.filter(product => product.checked).length === 0
);
  

// ALL 影響全部
watch(ALLcheckbox, (CB) => {
  products.value.forEach((product) => {
    product.checked = CB;
  });
});


// 勾選框全選與否 ALL的變化
watch(products, () => {
  const allChecked = products.value.every((product) => product.checked); // 確認是否全選
  if (ALLcheckbox.value !== allChecked) {
    ALLcheckbox.value = allChecked; // 僅在狀態不一致時更新
  }
}, { deep: true });

</script>



<style lang="sass">
@import './style.sass'
</style>
