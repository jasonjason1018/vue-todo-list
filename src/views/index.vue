<template>
    <div class="todo-container">
        <el-card class="todo-card" shadow="hover">
            <el-button type="info" v-for="label in labels" :key="label" @click="switchTab(label)">
                {{ label.label }}
            </el-button>
            <h1 class="title">
                🚀 {{ tabName }}
            </h1>
            <div class="input-section" v-if="tab === 'processing'">
                <el-input
                    v-model="newTask"
                    placeholder="輸入任務..."
                    @keyup.enter="addTask"
                    clearable
                >
                </el-input>
                <el-button width="100%" type="primary" @click="addTask">
                    <el-icon><Plus /></el-icon>
                </el-button>
            </div>
            <el-divider></el-divider>
            <el-empty v-if="tasks.length === 0" description="沒有任務，來點新挑戰吧！" />
            <el-scrollbar v-else class="task-list">
                <el-card
                    v-for="(task, index) in tasks"
                    :key="index"
                    class="task-item"
                    :class="{ completed: task.completed }"
                    shadow="always"
                >
                    <div class="task-content">
                        <span @click="toggleComplete(index)">{{ task.text }}</span>
                        <div class="task-actions">
                            <el-tag
                                v-if="task.completed"
                                type="success"
                                effect="dark"
                            >
                                已完成
                            </el-tag>
                            <el-button
                                type="danger"
                                size="small"
                                @click="deleteTask(index)"
                            >
                                <el-icon><Delete /></el-icon>
                            </el-button>
                        </div>
                    </div>
                </el-card>
            </el-scrollbar>
        </el-card>
    </div>
</template>
  
<script setup>
    import { ref, onMounted } from 'vue';
    import "@/assets/index.css";
    import { Delete, Plus } from '@element-plus/icons-vue';
    import axios from 'axios';

    const labels = ref([
        {
            label: '待處理',
            type: 'processing'
        },
        {
            label: '已完成',
            type: 'completed'
        },
        {
            label: '待確認',
            type: 'pending'
        }
    ]);

    const tab = ref('processing');
    const tabName = ref('待處理');

    const switchTab = (data) => {
        tab.value = data.type;
        tabName.value = data.label;
    }

    
    const newTask = ref('')
    const tasks = ref([])

    const addTask = () => {
        if (newTask.value.trim() !== '') {
            tasks.value.push({ text: newTask.value, completed: false })
            newTask.value = ''
        }
    }

    const toggleComplete = (index) => {
        tasks.value[index].completed = !tasks.value[index].completed
    }

    const deleteTask = (index) => {
        tasks.value.splice(index, 1)
    }
</script>