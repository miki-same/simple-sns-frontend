<script setup lang="ts">
import {onBeforeMount, onMounted, reactive, ref} from 'vue'
import axios from 'axios'

const posts=ref<Array<typeOfPosts>|undefined>();

type typeOfPosts = {
  message: string
  reply_for: number | null
  post_id: number
  posted_at: number
  posted_by: number
}

type typeOfPostCreate = {
  Message: string
}

onMounted(() => {
  const endPoint: string = 'https://sns-fastapi-eidnhgfbzq-an.a.run.app/posts';
  axios.get(
    endPoint
  ).then(
    (response) => {
      posts.value=response.data;
      for (const post of posts.value) {
        console.log(post.message);
      }
    }
  )
})

const dummyPosts = [
  {
    "message": "こんにちは!初めて投稿します。",
    "reply_for": null,
    "post_id": 1,
    "posted_at": 1672138696.5741038,
    "posted_by": 1
  },
  {
    "message": "DaisyUI最高！一番好きなUIフレームワークです\nTailwindCSS最高！最高！",
    "reply_for": null,
    "post_id": 1,
    "posted_at": 1672138696.5741038,
    "posted_by": 1
  },
  {
    "message": "Me too!",
    "reply_for": null,
    "post_id": 1,
    "posted_at": 1672138696.5741038,
    "posted_by": 1
  },
]



</script>

<template>
<div class="w-full mx-auto">
  <header>
    <div class="navbar bg-base-100">
      <div class="flex-none">
        <button class="btn btn-square btn-ghost">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-5 h-5 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path></svg>
        </button>
      </div>
      <div class="flex-1">
        <a class="btn btn-ghost normal-case text-xl">Simplest</a>
      </div>
      <div class="flex-none">
        <button class="btn btn-square btn-ghost">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-5 h-5 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 12h.01M12 12h.01M19 12h.01M6 12a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0z"></path></svg>
        </button>
      </div>
      <div class="flex-none">
        <button class="btn">ログイン</button>
      </div>
      <div class="flex-none">
        <button class="btn">新規登録</button>
      </div>
    </div>
  </header>
  <!-- The button to open modal -->
  <label for="my-modal" class="btn mb-4">open modal</label>

  <!-- Put this part before </body> tag -->
  <input type="checkbox" id="my-modal" class="modal-toggle" />
  <div class="modal">
    <div class="modal-box">
      <h3 class="font-bold text-lg">SimpleSNSへようこそ!!</h3>
      <p class="py-4">さっそく何か投稿してみましょう。</p>
      <p>あなたの話を誰かが聞いてくれます！</p>
      <div class="modal-action">
        <label for="my-modal" class="btn">わかった!</label>
      </div>
    </div>
  </div>
  <form>
    <input type="text" placeholder="Type here" class="input input-bordered input-primary w-full max-w-xs mb-4" />
    <button class="btn btn-primary">投稿</button>
  </form>
  <div class="container">
  <ul>
    <li v-for="post in posts" :key="post.post_id">
      <div class="card w-full bg-base-100 shadow-xl mx-auto mb-3">
      <div class="card-body">
        <h2 class="card-title">{{ post.posted_by }}</h2>
        <p class="text-lg">{{ post.message }}</p>
      <div class="card-actions justify-end">
        <button class="btn btn-primary">返信</button>
        <label class="swap swap-rotate text-5xl">
  
        <!-- this hidden checkbox controls the state -->
        <input type="checkbox" />
        <div class="swap-on">💖</div>
        <div class="swap-off">🤍</div>
        </label>

      </div>
      </div>
    </div>
    </li>
  </ul>
  </div>
</div>
</template>