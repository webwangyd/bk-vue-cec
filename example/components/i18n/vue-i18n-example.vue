<!--
 * Tencent is pleased to support the open source community by making
 * 科技内在设计（T-inside） available.
 *
 * Copyright (C) 2021 TID Limited, a DAO.  All rights reserved.
 *
 * 科技内在设计（T-inside） is licensed under the MIT License.
 *
 * License for 科技内在设计（T-inside）:
 *
 *
 * Terms of the MIT License:
 * ---------------------------------------------------
 * Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
 * documentation files (the "Software"), to deal in the Software without restriction, including without limitation
 * the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and
 * to permit persons to whom the Software is furnished to do so, subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in all copies or substantial portions of
 * the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT
 * THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF
 * CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
 * IN THE SOFTWARE.
-->

<template>
    <div>
        <h2>
            <i href="javascript:void(0)" class="back-icon bk-icon icon-arrows-left" @click="back"></i>
            结合 vue-i18n@6+ 使用
        </h2>
        <div class="example-item" style="height: 400px">
            <bk-date-picker class="mr15" :type="'datetime'" :open="true" v-model="initDateTime"></bk-date-picker>
            <bk-button type="primary submit" title="切换中英文" @click="handleClick">
                切换中英文
            </bk-button>
            <span class="bk-text-primary">当前为{{curLang === 'zhCN' ? '中文' : '英文'}}</span>
        </div>
    </div>
</template>
<script>
    import Vue from 'vue'
    import VueI18n from 'vue-i18n'
    import { bkDatePicker, bkButton, locale, lang } from '@'

    Vue.use(VueI18n)

    // 设置 bk-vue-cec 的语言包到 VueI18n 中
    const messages = {
        zhCN: Object.assign(lang.zhCN),
        enUS: Object.assign(lang.enUS)
    }

    // 获取当前的语言
    const curLocale = localStorage.getItem('curLang') || 'zhCN'

    // 实例化 VueI18n 对象
    const i18n = new VueI18n({
        locale: curLocale,
        fallbackLocale: 'zhCN',
        messages,
        missing (locale, path) {
            const parsedPath = i18n._path.parsePath(path)
            return parsedPath[parsedPath.length - 1]
        }
    })

    // 设置 bk-vue-cec 中 i18n 的处理函数，这句的意思是把 bk-vue-cec 中 i18n 的处理设置为 vue-i18n 的处理。
    // 如果和 vue-i18n 一起使用，那么这一句是必须的。
    locale.i18n((key, value) => i18n.t(key, value))

    export default {
        components: {
            bkDatePicker,
            bkButton
        },
        data () {
            return {
                initDateTime: new Date(),
                curLang: ''
            }
        },
        created () {
            // 获取语言标识
            this.curLang = localStorage.getItem('curLang') || 'zhCN'
        },
        methods: {
            back () {
                window.location.href = '#/i18n?anchor=jie-he-vue-i18n-at-6-shi-yong-vue-i18n-example'
            },
            handleClick (event) {
                // 当前为中文时
                if (this.curLang === 'zhCN') {
                    localStorage.setItem('curLang', 'enUS')
                } else { // 当前为英文时
                    localStorage.setItem('curLang', 'zhCN')
                }
                this.$nextTick(() => {
                    window.location.reload()
                })
            }
        }
    }
</script>
