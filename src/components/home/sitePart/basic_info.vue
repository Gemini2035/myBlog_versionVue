<!--
 * @Date: 2023-11-07 20:08:39
 * @LastEditors: Gemini2035 76091679+Gemini2035@users.noreply.github.com
 * @LastEditTime: 2023-11-11 20:48:07
 * @FilePath: /myBlog_versionVue/src/components/home/sitePart/basic_info.vue
-->
<script lang="ts" setup>
// 上线时间 使用语言 语言占比 左右2:1 左上下1:1
import Languagepercentage from './basicInfo/language_percentage.vue';
import Timeline from './basicInfo/timeline.vue';
import Structure from './basicInfo/structure.vue';
</script>

<template>
    <div class="basic-container">
        <div class="left-part">
            <div class="time-content">
                <Languagepercentage />
            </div>
            <div class="structure-content">
                <Structure />
            </div>
        </div>
        <div class="right-part">
            <Timeline />
        </div>
    </div>
</template>

<style lang="less" scoped>
.basic-container {
    width: 100%;
    height: 100%;
    display: flex;
    border-bottom: 1px solid var(--ms-black);

    .left-part {
        width: 60%;
        height: 100%;

        .time-content {
            width: 100%;
            height: 60%;
            position: relative;
        }

        .structure-content {
            width: 100%;
            height: calc(40% - 1px);
            border-top: 1px solid;
        }
    }

    .right-part {
        width: calc(40% - 1px);
        height: 100%;
        border-left: 1px solid;
    }
}
</style>