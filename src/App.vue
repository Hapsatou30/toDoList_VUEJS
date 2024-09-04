<template>
  <!-- Inclusion de la feuille de style Font Awesome pour utiliser les icônes -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">

  <h1>La liste des tâches à faire</h1>
  <hr>

  <!-- Formulaire pour ajouter une nouvelle tâche -->
  <form action="" @submit.prevent="addTask">
    <div class="mb-3">
      <label for="task">Tâche</label>
      <!-- Champ pour le titre de la tâche, lié au modèle newTask.task -->
      <input type="text" class="form-control" id="task" v-model="newTask.task" placeholder="Ajouter une tâche">
    </div>
    <div class="mb-3">
      <label for="description">Description</label>
      <!-- Champ pour la description de la tâche, lié au modèle newTask.description -->
      <textarea class="form-control" id="description" v-model="newTask.description" placeholder="Ajouter une description"></textarea>
    </div>
    <!-- Bouton pour soumettre le formulaire, désactivé si les champs ne sont pas remplis -->
    <button class="btn btn-primary" :disabled="!formValid">Ajouter une tâche</button>
  </form>
  <hr>

  <!-- Tableau pour afficher la liste des tâches -->
  <table class="table">
    <thead>
      <tr>
        <th>ID</th>
        <th>Tâche</th>
        <th>Description</th>
        <th>Date de création</th>
        <th>Actions</th>
      </tr>
    </thead>
   
    <tbody>
      <!-- Boucle pour afficher chaque tâche -->
      <tr v-for="task in tasks" :key="task.id" :class="{ completed: task.completed }">
        <td>{{ task.id }}</td>
        <td>{{ task.task }}</td>
        <td>{{ task.description }}</td>
        <td>{{ task.created_at }}</td>
        <td>
          <!-- Bouton pour supprimer une tâche, avec une icône de corbeille -->
          <button @click="deleteTask(task.id)" style="color: red" class="mx-5">
            <i class="fas fa-trash"></i>
          </button>
          <!-- Case à cocher pour marquer une tâche comme terminée -->
          <input type="checkbox" v-model="task.completed">
        </td>
      </tr>
    </tbody>
  </table>

  <!-- Message affiché lorsque la liste des tâches est vide -->
  <div v-if="tasks.length === 0" style="margin-top: 20px;">
    <h5>Vous n'avez pas de tâches à faire</h5>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

// Tableau pour stocker la liste des tâches
const tasks = ref([]);

// Objet pour stocker les informations de la nouvelle tâche
const newTask = ref({ task: '', description: '' });

// Fonction pour ajouter une nouvelle tâche à la liste
const addTask = () => {
  // Vérification que les champs "task" et "description" ne sont pas vides
  if (newTask.value.task && newTask.value.description) {
    const task = {
      ...newTask.value,
      id: tasks.value.length + 1,  // Génération de l'ID unique pour la tâche
      created_at: new Date().toLocaleString(),  // Enregistrement de la date de création
      completed: false  // Initialisation de la tâche comme non terminée
    };
    // Ajout de la nouvelle tâche à la liste des tâches
    tasks.value.push(task);
    // Réinitialisation des champs du formulaire
    newTask.value = { task: '', description: '' };
  }
};

// Calcul pour valider le formulaire
// Le bouton "Ajouter une tâche" sera désactivé tant que ces deux champs ne seront pas remplis
const formValid = computed(() => newTask.value.task && newTask.value.description);

// Fonction pour supprimer une tâche de la liste par son ID
const deleteTask = (taskId) => {
  tasks.value = tasks.value.filter((task) => task.id !== taskId);
};
</script>

<style>
/* Style pour les tâches terminées : texte barré et opacité réduite */
.completed {
  text-decoration: line-through;
  opacity: .5;
}
</style>
