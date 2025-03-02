<template>
    <div class="flex flex-col justify-center items-center">

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">输入链接</div>
        <form @submit.prevent="submitForm">
            <div class="form-control">
                <div class="join">
                    <input type="text" placeholder="小红书/网易云音乐/QQ音乐/汽水音乐/微博分享链接"
                        class="input input-bordered join-item input-success w-full" v-model="url" required />
                    <button class="btn join-item rounded-r-full" type="submit" :disabled="loading">
                        <span v-if="loading" class="loading loading-spinner loading-md"></span>
                        <span v-else>Go</span>
                    </button>
                </div>
            </div>
        </form>

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">解析结果</div>
        <div class="alert" v-if="response.code == undefined || response.code == null">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                class="stroke-info shrink-0 w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
            <span>在上面输入分享链接后戳Go即可处理。<br>祝各位新年快乐，25年大吉大利！</span>
        </div>
        <div class="alert alert-success" v-else-if="response.code === 1">
            <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current shrink-0 h-6 w-6" fill="none"
                viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
            <span>处理成功！</span>
            <details class="dropdown">
                <summary class="btn m-1">赞助服务</summary>
                <ul class="menu dropdown-content bg-base-100 rounded-box z-[1] w-52 p-2 shadow">
                    <li><button onclick="my_modal_199.showModal()">￥1.99</button></li>
                    <dialog id="my_modal_199" class="modal">
                        <div class="modal-box">
                            <img src="../assets/image/1.99.jpg">
                            <div class="modal-action">
                                <form method="dialog">
                                    <button class="btn">关闭</button>
                                </form>
                            </div>
                        </div>
                    </dialog>
                    <li><button onclick="my_modal_520.showModal()">￥5.20</button></li>
                    <dialog id="my_modal_520" class="modal">
                        <div class="modal-box">
                            <img src="../assets/image/5.20.jpg">
                            <div class="modal-action">
                                <form method="dialog">
                                    <button class="btn">关闭</button>
                                </form>
                            </div>
                        </div>
                    </dialog>
                    <li><button onclick="my_modal_999.showModal()">￥9.99</button></li>
                    <dialog id="my_modal_999" class="modal">
                        <div class="modal-box">
                            <img src="../assets/image/9.99.jpg">
                            <div class="modal-action">
                                <form method="dialog">
                                    <button class="btn">关闭</button>
                                </form>
                            </div>
                        </div>
                    </dialog>
                </ul>


            </details>
            <a target="_blank" :href="'https://nclgclub.com/outgoing?url=' + response.data" class="btn">
                点我跳转
            </a>
        </div>
        <div v-else-if="response.code === 0">
            <div class="alert alert-error">
                <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current shrink-0 h-6 w-6" fill="none"
                    viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                <span>{{ response.message }}</span>
            </div>
        </div>

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">一些帮助</div>
        <div>
            网站的设计应该是比较易用的，如果你在使用上还是遇到了一些比较棘手的问题，可以进行查阅
            <label for="my-modal" class="btn btn-xs btn-success mx-2">更加详细的文档</label>。<br><br>
            <input type="checkbox" id="my-modal" class="modal-toggle" />
            <div class="modal">
                <div class="modal-box">
                    <img src="../assets/qr_code.png">
                    <div class="modal-action">
                        <label for="my-modal" class="btn btn-sm">
                            好
                        </label>
                    </div>
                </div>
            </div>

            现已支持处理以下分享链接：
            <li class="italic">微博</li>
            <li class="italic">小红书</li>
            <li class="italic">汽水音乐</li>
            <li class="italic">网易云音乐</li>
            <li class="italic">QQ音乐（用户主页需要登陆自己账号后才能查看）</li>

            若出现任何问题，可以通过公众号进行反馈以获得帮助。<br>当然,也欢迎闲聊～
        </div>

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">免责声明</div>
        <p>本站点仅供学习测试，完全免费，希望能帮你找到那个喜欢的TA！</p>
        <p> 一切分享性质以及使用过程中导致的责任仅与使用者相关，若不同意请勿使用。</p>

    </div>
</template>

<script>
alert("希望你使用这个工具是为了爱情而不是利益，为了防止某些坏人滥用，请通过微信公众号打开使用！");

export default {
    data() {
        return {
            url: '',
            loading: false,
            response: {},
        };
    },
    methods: {
        async submitForm() {
            if (!navigator.userAgent.includes('MicroMessenger')) {
                alert('滥用的人太多，请在微信公众号中打开使用！');
                return;
            }

            this.loading = true;
            this.response = {}; // 清空响应数据
            await new Promise(resolve => setTimeout(resolve, 1666));
            try {
                const shareUrl = this.extractUrl(this.url);
                if (!shareUrl) {
                    alert('未检测到有效链接，请重新输入！');
                    this.loading = false;
                    return;
                }
                const response = await this.fetchParsedUrl(shareUrl);
                this.response = await response.json();
                this.url = ''; // 清空输入框
            } catch (error) {
                console.error(error);
                this.response = { code: 0, message: '可以前往公众号加群寻求帮助。' };
            } finally {
                this.loading = false;
            }
        },
        extractUrl(text) {
            const urlPattern = /(https?:\/\/[^\s]+)/g;
            const urls = text.match(urlPattern);
            return urls ? urls[0] : null;
        },
        async fetchParsedUrl(shareUrl) {
            return fetch('/api/parse', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ shareUrl }),
            });
        }
    },
};
</script>
