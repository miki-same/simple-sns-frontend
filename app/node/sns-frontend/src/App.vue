<script setup lang="ts">
import { onBeforeMount, onMounted, reactive, ref } from 'vue'
import axios from 'axios'

const posts = ref<Array<typeOfPosts>>([]);
const loginForm = reactive<loginState>({
  username: "",
  password: ""
});

const checkLogin = () => {
  return localStorage.getItem('bearer_token') !== null;
}

const getCurrentUserName = ():string => {
  const currentUserName = localStorage.getItem('username');
  return currentUserName!==null ? currentUserName : "";
}

const isLogin = ref<boolean>(checkLogin());
const username = ref<string>(getCurrentUserName());

interface loginState {
  username: string
  password: string
}

const registerForm = reactive<registerState>({
  username: "",
  nickname: "",
  email: "",
  password: ""
})

interface registerState {
  username: string
  nickname: string
  email: string
  password: string
}

const createPostForm = reactive<createPostState>({
  message: ""
})

interface createPostState {
  message: string
}

type typeOfPosts = {
  message: string
  reply_for: number | null
  post_id: number
  posted_at: number
  posted_by: number
}



onMounted(() => {
  console.log(isLogin);
  getAllPosts();
})

const getAllPosts = () => {
  const endPoint: string = 'https://sns-fastapi-eidnhgfbzq-an.a.run.app/posts';
  axios.get(
    endPoint
  ).then(
    (response) => {
      posts.value = response.data.reverse();
      for (const post of posts.value) {
        console.log(post.message);
      }
    }
  )
}

const getUserName = (userId: number) => {
  const animals = ['パンダ', 'ワニ', 'キツネ', 'クマ', 'ヒツジ', 'サル', 'タコ'];
  const idx = (userId * 10520184081 + userId * 455955227) % animals.length;
  return '匿名' + animals[idx];
}

const foo = () => {
  console.log("posted!");
}


const unixTimeToDate = (time: number) => {
  let dateTime = new Date(time * 1000);
  return dateTime.toLocaleDateString('ja-JP') + ' ' + dateTime.toLocaleTimeString('ja-JP');
}

const login = () => {
  console.log('login');
  const params = new URLSearchParams();
  params.append('username', loginForm.username);
  params.append('password', loginForm.password);
  let config = {
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded'
    }
  };
  axios.post('https://sns-fastapi-eidnhgfbzq-an.a.run.app/token', params, config)
    .then(res => {
      alert("ログイン成功！");
      console.log(res);
      localStorage.setItem('bearer_token', res.data.access_token);
      isLogin.value = true;
      axios.get('https://sns-fastapi-eidnhgfbzq-an.a.run.app/users/me',
        {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("bearer_token")}`
          }
        }
      )
        .then(res => {
          console.log("ユーザー情報を取得");
          localStorage.setItem('username', res.data.username);
          username.value = res.data.username;
        })
        .catch(e => {
          console.log("ユーザー情報取得に失敗しました");
        })
    })
    .catch(e => {
      alert("ログインに失敗しました");
      console.log(e);
    })
}

const logout = () => {
  isLogin.value = false;
  localStorage.removeItem("username");
  localStorage.removeItem("bearer_token");
}

const register = () => {
  axios.post('https://sns-fastapi-eidnhgfbzq-an.a.run.app/users',
    {
      username: registerForm.username,
      nickname: registerForm.nickname,
      email: registerForm.email,
      password: registerForm.password
    })
    .then(res => {
      alert('登録成功！');
      console.log(res);
    })
    .catch(e => {
      alert("登録に失敗しました");
      console.log(e);
    })
}

const createPost = () => {
  if (!(localStorage.getItem("bearer_token"))) {
    alert('ログインしてください！');
  }
  else if (createPostForm.message === "") {
    alert('本文を入力してください！');
  } else {
    axios.post('https://sns-fastapi-eidnhgfbzq-an.a.run.app/posts',
      {
        message: createPostForm.message,
      }, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem("bearer_token")}`
      }
    }
    ).then(res => {
      createPostForm.message = "";
      getAllPosts();
    }).catch(e => {
      alert("投稿に失敗しました");
      console.log(e);
    })
  }
}
</script>

<template>
  <div class="w-full mx-auto">

    <header>
      <div class="navbar bg-base-100">
        <div class="flex-none">
          <label for="my-drawer" class="btn btn-square btn-ghost drawer-button">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
              class="inline-block w-5 h-5 stroke-current">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
            </svg>
          </label>
        </div>
        <div class="flex-1">
          <a class="btn btn-ghost normal-case text-xl">Simplest</a>
        </div>
        <div v-if="isLogin">
          <p class="text-xl text-blue-600">{{ username }}</p>
          <!-- The button to open modal -->
          <label for="login-btn" class="btn flex-none">ログアウト</label>
          <!-- Put this part before </body> tag -->
          <input type="checkbox" id="login-btn" class="modal-toggle" />
          <div class="modal">
            <div class="modal-box">
              <h4 class="normal-case text-2xl font-midium">ログアウトしますか？</h4>
              <button class="btn btn-primary w-full max-w-xs" @click="logout">ログアウト</button>
              <div class="modal-action">
                <label for="login-btn" class="btn">閉じる</label>
              </div>
            </div>
          </div>
        </div>
        <div v-else>
          <!-- The button to open modal -->
          <label for="login-btn" class="btn flex-none">ログイン</label>
          <!-- Put this part before </body> tag -->
          <input type="checkbox" id="login-btn" class="modal-toggle" />
          <div class="modal">
            <div class="modal-box">
              <h4 class="normal-case text-2xl font-midium">SimpleSNSへログイン</h4>
              <!--TODO: ログインを実装 ログインボタンを閉じるボタンに変更-->
              <input type="text" placeholder="ID" class="input input-bordered w-full max-w-xs"
                v-model="loginForm.username" />
              <input type="password" placeholder="Password" class="input input-bordered w-full max-w-xs"
                v-model="loginForm.password" />
              <button class="btn btn-primary w-full max-w-xs" @click="login">ログイン</button>
              <div class="modal-action">
                <label for="login-btn" class="btn">閉じる</label>
              </div>
            </div>
          </div>

          <!-- The button to open modal -->
          <label for="register-btn" class="btn flex-none">新規登録</label>
          <!-- Put this part before </body> tag -->
          <input type="checkbox" id="register-btn" class="modal-toggle" />
          <div class="modal">
            <div class="modal-box">
              <h4 class="normal-case text-2xl font-midium">SimpleSNSへ登録</h4>
              <input type="text" placeholder="username" class="input input-bordered w-full max-w-xs"
                v-model="registerForm.username" />
              <input type="text" placeholder="nickname" class="input input-bordered w-full max-w-xs"
                v-model="registerForm.nickname" />
              <input type="text" placeholder="e-mail" class="input input-bordered w-full max-w-xs"
                v-model="registerForm.email" />
              <input type="password" placeholder="Password" class="input input-bordered w-full max-w-xs"
                v-model="registerForm.password" />
              <button class="btn btn-primary w-full max-w-xs" @click="register">新規登録</button>
              <div class="modal-action">
                <label for="register-btn" class="btn">閉じる</label>
              </div>
            </div>
          </div>
        </div>

      </div>
    </header>

    <div class="drawer">
      <input id="my-drawer" type="checkbox" class="drawer-toggle" />
      <div class="drawer-content">

        <body class="my-3">

          <textarea placeholder="Type your thoughts!" class="textarea textarea-primary w-96 h-28 max-w-xs mb-8"
            v-model="createPostForm.message"></textarea>
          <button class="btn btn-primary" v-on:click="createPost">投稿</button>

          <div class="container">
            <ul>
              <li v-for="post in posts" :key="post.post_id">
                <div class="card w-full bg-base-100 shadow-xl mx-auto mb-3">
                  <div class="card-body">
                    <h2 class="card-title">{{ getUserName(post.posted_by) }}</h2>
                    <p class="text-lg">{{ post.message }}</p>
                    <p>{{ unixTimeToDate(post.posted_at) }}</p>
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
        </body>

      </div>
      <div class="drawer-side">
        <label for="my-drawer" class="drawer-overlay"></label>
        <ul class="menu p-4 w-80 bg-base-100 text-base-content">
          <!-- Sidebar content here -->
          <li><a>フォロー</a></li>
          <li><a>フォロワー</a></li>

        </ul>
      </div>
    </div>

  </div>
</template>