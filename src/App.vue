<template>
    <div id="app">
        <keep-alive>
            <router-view :key="$route.path" />
        </keep-alive>
    </div>
</template>