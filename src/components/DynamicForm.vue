<template>
    <div class="form-container">
      <input 
        type="text" 
        v-model="searchText" 
        placeholder="Search..." 
        class="search-input"
        @input="highlightSearch" 
      />
      <form @submit.prevent>
        <div 
          v-for="(field, index) in fields" 
          :key="field.id" 
          :class="{'form-field': true, 'highlight': field.highlight}"
        >
          <input 
            type="text" 
            v-model="field.value" 
            class="text-input"
            @input="() => countVowels(field)" 
          />
          <span class="vowel-count">Vowels: {{ field.vowelCount }}</span>
          <button 
            type="button" 
            class="remove-button"
            @click="removeField(index)" 
            v-if="fields.length > 1"
          >
            Remove
          </button>
        </div>
        <button 
          type="button" 
          class="add-button"
          @click="addField" 
          :disabled="fields.length >= 10"
        >
          Add Field
        </button>
      </form>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref, reactive, watch } from 'vue';
  
  interface Field {
    id: number;
    value: string;
    vowelCount: number;
    highlight: boolean;
  }
  
  export default defineComponent({
    setup() {
      const searchText = ref<string>('');
      const fields = reactive<Field[]>([
        { id: 1, value: '', vowelCount: 0, highlight: false },
        { id: 2, value: '', vowelCount: 0, highlight: false },
        { id: 3, value: '', vowelCount: 0, highlight: false }
      ]);
  
      const addField = () => {
        const newId = fields.length + 1;
        fields.push({ id: newId, value: '', vowelCount: 0, highlight: false });
      };
  
      const removeField = (index: number) => {
        fields.splice(index, 1);
      };
  
      const countVowels = (field: Field) => {
        const vowels = field.value.match(/[aeiou]/gi);
        field.vowelCount = vowels ? vowels.length : 0;
      };
      const highlightSearch = () => {
        fields.forEach(field => {
         if (searchText.value.trim() !== '' && field.value.trim() !== '') {
            field.highlight = field.value.includes(searchText.value);
          } else {
            field.highlight = false;
          }
        });
      };
  
      watch(searchText, highlightSearch);
  
      return {
        searchText,
        fields,
        addField,
        removeField,
        countVowels,
        highlightSearch
      };
    }
  });
  </script>
  
  <style>
  .form-container {
    width: 350px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 10px;
    background-color: #f9f9f9;
  }
  
  .search-input {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
  }
  
  .form-field {
    margin-bottom: 10px;
    display: flex;
    align-items: center;
  }
  
  .text-input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-right: 10px;
    box-sizing: border-box;
  }
  
  .vowel-count {
    margin-right: 10px;
  }
  
  .remove-button,
  .add-button {
    padding: 5px 10px;
    border: none;
    border-radius: 5px;
    background-color: #007bff;
    color: white;
    cursor: pointer;
  }
  
  .remove-button {
    background-color: #dc3545;
  }
  
  .remove-button:hover,
  .add-button:hover {
    opacity: 0.8;
  }
  
  .remove-button:disabled,
  .add-button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  
  .highlight {
    background-color: #d4edda;
  }
  
  .highlight .text-input {
    background-color: #c3e6cb;
  }
  </style>
  