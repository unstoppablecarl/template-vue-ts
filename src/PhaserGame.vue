<script setup lang="ts">
import { onMounted, onUnmounted, shallowRef } from 'vue';
import { EventBus } from './game/EventBus';
import StartGame from './game/main';
import * as Phaser from 'phaser';
import { type Game, type Scene } from 'phaser';

// Save the current scene instance
const scene = shallowRef<Scene>();
const game = shallowRef<Game>();

const emit = defineEmits(['current-active-scene']);

onMounted(() => {

    game.value = StartGame('game-container');
    
    EventBus.on('current-scene-ready', (scene_instance: Phaser.Scene) => {
        
        emit('current-active-scene', scene_instance);
    
        scene.value = scene_instance;
    
    });

});

onUnmounted(() => {

    if (game.value)
    {
        game.value.destroy(true);
        // @ts-expect-error: destroying instance
        game.value = null;
    }

});

defineExpose({ scene, game });

</script>

<template>
    <div id="game-container"></div>
</template>