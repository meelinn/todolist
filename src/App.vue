
<template>
  <div class="container">
    <div class="row">
 <div class="todo col-6">
  <h2>TO DO LIST</h2>
  <p class="border border-buttom "></p>
  <template v-for="(toDoItem,i) in toDoArray" :key="i">
<div class="form-check">

  <input class="form-check-input" type="checkbox" :value="toDoItem" id="flexCheckDefault" ref="checkboxRef" @change="toggleCheck(i,$event)">   
  <label class="form-check-label" for="flexCheckDefault">
    {{ toDoItem }}
  </label>
  <button class="btn py-0 pb-1" @click="deleteItem(i,'toDoArray')">
  <i class="bi bi-x"></i></button>
</div>
</template>
<input v-show="isAdd" @blur="addItem()" @keyup.enter="addItem()"  ref="addInputRef" class="form-control" type="text" placeholder="Take a note...">
<p class="text-danger" v-show="isError">This TO DO already exists!</p>
<button class="btn btn-primary" @click="showAddInput()">+Add Item</button>
</div>
<div class="done col-6">
  <h2>DONE</h2>
  <p class="border border-buttom "></p>
  <template v-for="(doneItem,i) in doneArray" :key="i">
<div class="form-check">
  <input class="form-check-input" type="checkbox" :value="doneItem" id="flexCheckDefault" checked  ref="checkboxRef"  @change="toggleCheck(i,$event)">
  <label class="form-check-label" for="flexCheckDefault">
    {{ doneItem }}
  </label>
  <button class="btn py-0 pb-1" @click="deleteItem(i,'doneArray')">
    <i class="bi bi-x"></i></button>
</div>
</template>
</div>
</div>
</div>
</template>

<script setup lang="ts">
import { ref,nextTick  } from "vue";
const toDoArray=ref<string[]>([]);
const doneArray=ref<string[]>([]);
const isAdd=ref<boolean>(false);
const addInputRef = ref<HTMLInputElement | null>(null);
const checkboxRef= ref<HTMLInputElement | null>(null);
const isError=ref<boolean>(false);

function showAddInput(){
  isAdd.value=true;
  nextTick(()=>{
    addInputRef.value.focus();
  })

}

function addItem(){
  isError.value = false;
  
  if (!toDoArray.value.includes(addInputRef.value.value)){
    if(addInputRef.value.value !== "" ){
    isAdd.value=false;
    toDoArray.value.push(addInputRef.value.value
    );
    addInputRef.value.value="";
  }}
  else{
    isError.value = true;
  }
}


function toggleCheck(i,event){
  if(event.target.checked){
    doneArray.value.push(event.target.value);
    toDoArray.value.splice(i,1)
    event.target.checked=false;
    console.log('donearray',doneArray.value);
  }else{
    toDoArray.value.push(event.target.value);
    doneArray.value.splice(i,1)
    event.target.checked=true;
    console.log('todoarray',toDoArray.value);
  }
}

function deleteItem(i,arrayName){
switch (arrayName) {
  case 'toDoArray':
    return  toDoArray.value.splice(i, 1);
    break;
  case 'doneArray':
    return doneArray.value.splice(i, 1);
    break;
}
}
</script>
