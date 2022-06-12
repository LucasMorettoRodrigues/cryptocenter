<template>
    <div>
        <div class="flex">
            <h2>Top Gainers</h2>
            <RouterLink to="/">More ></RouterLink>
        </div>
        <div>
            <ul>
                <li class="flex">
                    <div>1</div>
                    <div>Name</div>
                    <div>15%</div>
                </li>
                <li class="flex">
                    <div>2</div>
                    <div>Name</div>
                    <div>15%</div>
                </li>
                <li class="flex">
                    <div>3</div>
                    <div>Name</div>
                    <div>15%</div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import { RouterLink } from 'vue-router'
</script>