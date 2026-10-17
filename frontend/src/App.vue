<script setup>
import { computed, ref } from 'vue'
// 把類似 '2026-10-16T23:54' 的格式轉成 '2026-10-16 下午 11:54'
function formatDateTime(isoString) {
  if (!isoString) return ''
  // 處理有 T 的格式
  const dateObj = new Date(isoString)
  if (isNaN(dateObj.getTime())) return isoString // 如果不是標準格式就原樣回傳

  const year = dateObj.getFullYear()
  const month = String(dateObj.getMonth() + 1).padStart(2, '0')
  const day = String(dateObj.getDate()).padStart(2, '0')
  
  let hours = dateObj.getHours()
  const minutes = String(dateObj.getMinutes()).padStart(2, '0')
  
  // 判斷上下午並轉為 12 小時制
  const period = hours >= 12 ? '下午' : '上午'
  hours = hours % 12
  hours = hours ? hours : 12 // 如果是 0 點要顯示 12
  const strHours = String(hours).padStart(2, '0')

  return `${year}-${month}-${day} ${period} ${strHours}:${minutes}`
}
// 💡 1. 控制詳細資料浮動視窗的變數
const selectedItem = ref(null)

// 💡 2. 點擊卡片開啟浮動視窗的函式
function openDetail(item) {
  selectedItem.value = item
}

// 💡 3. 在浮動視窗裡執行的刪除函式
function deleteItem(id) {
  const confirmed = confirm('確定要刪除這筆失物紀錄嗎？')
  if (confirmed) {
    items.value = items.value.filter((item) => item.id !== id)
    selectedItem.value = null // 刪除後關閉彈窗
  }
}
const keyword = ref('')
const category = ref('全部')
const showForm = ref(false)


// 動態計算出現頻率最高的前 10 個分類標籤
const categories = computed(() => {
  // 1. 統計所有物品中每個分類出現了幾次
  const counts = {}
  items.value.forEach(item => {
    if (item.category) {
      counts[item.category] = (counts[item.category] || 0) + 1
    }
  })

  // 2. 把統計結果轉成陣列並按照出現次數由多到少排序
  const sortedCategories = Object.keys(counts).sort((a, b) => {
    return counts[b] - counts[a]
  })

  // 3. 取前 9 名（再加上固定的「全部」，總共最多 10 個標籤）
  const topCategories = sortedCategories.slice(0, 9)

  return ['全部', ...topCategories]
})

const items = ref([
  {
    id: 1,
    title: '白色無線耳機盒',
    category: '電子產品',
    location: '電腦教室',
    date: '2026-10-10',
    type: 'found',
    image:
      'https://images.unsplash.com/photo-1606220945770-b5b6c2c55bf1?w=600',
  },
  {
    id: 2,
    title: '黑色筆袋',
    category: '文具',
    location: '圖書館',
    date: '2026-10-09',
    type: 'lost',
    image:
      'https://images.unsplash.com/photo-1455390582262-044cdead277a?w=600',
  },
  {
    id: 3,
    title: '藍色水壺',
    category: '生活用品',
    location: '體育館',
    date: '2026-10-08',
    type: 'found',
    image:
      'https://images.unsplash.com/photo-1602143407151-7111542de6e8?w=600',
  },
])

// 新增表單用的雙向綁定資料
const now = new Date()
const formattedDate = `${now.getFullYear()}-${String(now.getMonth() + 1).padStart(2, '0')}-${String(now.getDate()).padStart(2, '0')} ${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}`

// 新增表單用的雙向綁定資料
const formTitle = ref('')
const formCategory = ref('')
const formLocation = ref('')
const formDate = ref(formattedDate) // 👈 這裡把原本寫死的日期改成帶入上面算好的時間變數！
const formType = ref('found')
const formImage = ref('')

const filteredItems = computed(() => {
  return items.value.filter((item) => {
    const matchesKeyword =
      `${item.title} ${item.location}`
        .toLowerCase()
        .includes(keyword.value.toLowerCase())

    const matchesCategory =
      category.value === '全部' ||
      item.category === category.value

    return matchesKeyword && matchesCategory
  })
})
// 📍 把這兩段分頁計算屬性貼在這裡！
const currentPage = ref(1)
const itemsPerPage = ref(12) // 每頁顯示 12 筆

// 計算總共有幾頁
const totalPages = computed(() => {
  return Math.ceil(filteredItems.value.length / itemsPerPage.value)
})

// 切出當前頁面要顯示的清單
const paginatedItems = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value
  const end = start + itemsPerPage.value
  return filteredItems.value.slice(start, end)
})
function submitItem() {
  let errors = []

  if (!formTitle.value.trim()) {
    errors.push('請填寫物品名稱！')
  }
  if (!formLocation.value.trim()) {
    errors.push('請填寫地點！')
  }
  if (!formCategory.value) {
    errors.push('請選擇物品分類！')
  }

  // 2. 如果 errors 裡面有東西，代表至少有一個以上沒填
  if (errors.length > 0) {
    // 用換行符號（\n）把所有錯誤訊息串在一起，一次全部跳出來！
    alert(errors.join('\n'))
    return // 擋在這裡，不往下執行新增
  }
items.value.unshift({
    id: Date.now(),
    title: formTitle.value,
    category: formCategory.value,
    location: formLocation.value,
    date: formatDateTime(formDate.value), // 👈 這裡用 formatDateTime 轉換格式！
    type: formType.value,
    image: formImage.value.trim(),
  })

  formTitle.value = ''
  formCategory.value = '' // 👈 這行就是關鍵：強制讓分類恢復成「請選擇物品分類」！
  formLocation.value = ''
  formType.value = 'found'
  formImage.value = ''
  location.reload()
}




</script>

<template>
  <div class="min-h-screen bg-slate-50 text-slate-800">
    <!-- 導覽列 -->
    <header class="border-b border-slate-200 bg-white">
      <div
        class="mx-auto flex max-w-6xl items-center
        justify-between px-5 py-4"
      >
        <a href="/" class="text-xl font-black text-indigo-600">
          ◈ 尋物雷達
        </a>

        <button
          class="rounded-xl bg-indigo-600 px-4 py-2
          text-sm font-bold text-white hover:bg-indigo-700"
          @click="showForm = true"
        >
          + 刊登物品
        </button>
      </div>
    </header>

    <main class="mx-auto max-w-6xl px-5 py-10">
      <!-- 主視覺 -->
      <section
        class="rounded-3xl bg-indigo-600 px-6 py-12
        text-white sm:px-12"
      >
        <p class="mb-3 text-sm font-bold tracking-widest text-indigo-200">
          CAMPUS LOST & FOUND
        </p>

        <h1 class="text-3xl font-black leading-tight sm:text-5xl">
          遺失的物品，<br />
          讓我們幫你找回。
        </h1>

        <p class="mt-4 max-w-lg text-sm leading-7 text-indigo-100">
          搜尋校園失物，或刊登拾獲物品。
          未來將透過 AI 影像比對，推薦可能的失物配對。
        </p>

        <div class="mt-7 flex max-w-xl flex-col gap-3 sm:flex-row">
          <input
            v-model="keyword"
            type="text"
            placeholder="搜尋耳機、水壺、圖書館..."
            class="min-w-0 flex-1 rounded-xl bg-white px-4
            py-3 text-slate-800 outline-none
            focus:ring-4 focus:ring-indigo-300"
          />

          <button
            class="rounded-xl bg-white px-6 py-3
            font-bold text-indigo-700 hover:bg-indigo-50"
            @click="category = '全部'"
          >
            搜尋物品
          </button>
        </div>
      </section>

      <!-- 分類 -->
      <section class="mt-10">
        <div class="flex flex-wrap gap-2">
          <button
            v-for="item in categories"
            :key="item"
            class="rounded-full border px-4 py-2 text-sm font-semibold
            transition"
            :class="
              category === item
                ? 'border-indigo-600 bg-indigo-600 text-white'
                : 'border-slate-200 bg-white text-slate-600 hover:border-indigo-300'
            "
            @click="category = item"
          >
            {{ item }}
          </button>
        </div>
      </section>

      <!-- 清單標題 -->
      <section class="mt-8">
        <div class="mb-5 flex items-end justify-between">
          <div>
            <h2 class="text-2xl font-black">校園失物列表</h2>
            <p class="mt-1 text-sm text-slate-500">
              找找看，有沒有你正在尋找的物品。
            </p>
          </div>

          <span class="text-sm text-slate-500">
            {{ filteredItems.length }} 筆紀錄
          </span>
        </div>

        <!-- 物品卡片 -->
        <div class="grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-3">
          <article
            v-for="item in paginatedItems"
            :key="item.id"
            class="overflow-hidden rounded-2xl border border-slate-200
            bg-white shadow-sm transition hover:-translate-y-1
            hover:shadow-lg cursor-pointer"
            @click="openDetail(item)"
          >
            <div class="relative h-52 bg-slate-100">
              <img
                v-if="item.image"
                :src="item.image"
                :alt="item.title"
                class="h-full w-full object-cover"
              />

              <div
                v-else
                class="flex h-full items-center justify-center
                text-slate-400"
              >
                尚無物品照片
              </div>

              <span
                class="absolute left-3 top-3 rounded-full px-3 py-1
                text-xs font-bold"
                :class="
                  item.type === 'found'
                    ? 'bg-emerald-100 text-emerald-700'
                    : 'bg-amber-100 text-amber-700'
                "
              >
                {{ item.type === 'found' ? '已拾獲' : '正在尋找' }}
              </span>
            </div>

            <div class="p-5">
              <p class="text-xs font-semibold text-indigo-600">
                {{ item.category }}
              </p>

              <h3 class="mt-2 text-lg font-bold">
                {{ item.title }}
              </h3>

              <div class="mt-4 space-y-2 text-sm text-slate-500">
                <p>📍 {{ item.location }}</p>
                <p>📅 {{ item.date }}</p>
              </div>

              <div
                class="mt-5 w-full text-center rounded-xl border border-indigo-200
                py-2.5 text-sm font-bold text-indigo-700
                hover:bg-indigo-50"
              >
                查看詳細內容
              </div>
            </div>
          </article>
        </div>
        <!-- 物品卡片網格 -->
        <div class="grid grid-cols-1 gap-5 sm:grid-cols-2 lg:grid-cols-3">
          <article
            v-for="item in paginatedItems"
            :key="item.id"
            
          >
            <!-- 卡片內容 -->
          </article>
        </div>

        <!-- ========================================== -->
        <!-- 📍 請把「分頁器區塊」貼在這裡！ -->
        <!-- ========================================== -->
        <div v-if="totalPages > 1" class="mt-10 flex justify-center items-center gap-2">
          <button :disabled="currentPage === 1" @click="currentPage--" class="px-4 py-2 border rounded-xl bg-white">&lt;</button>
          <button v-for="page in totalPages" :key="page" @click="currentPage = page" :class="currentPage === page ? 'bg-indigo-600 text-white' : 'bg-white border'" class="px-4 py-2 rounded-xl font-bold">
            {{ page }}
          </button>
          <button :disabled="currentPage === totalPages" @click="currentPage++" class="px-4 py-2 border rounded-xl bg-white">&gt;</button>
        </div>

        <!-- 原本就有的提示文字 -->
        <p
          v-if="filteredItems.length === 0"
          class="py-16 text-center text-slate-500"
        >
          找不到符合條件的物品，試試其他關鍵字。
        </p>
      </section>
    </main>

    <!-- 💡 點擊卡片後彈出的詳細資料浮動視窗 (Modal) -->
<div
      v-if="selectedItem"
      class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-5 backdrop-blur-xs"
      @click.self="selectedItem = null"
    >
      <div class="w-full max-w-md rounded-2xl bg-white p-6 shadow-2xl max-h-[90vh] overflow-y-auto">
        <div class="flex items-center justify-between mb-4">
          <span class="text-xs font-semibold text-indigo-600">{{ selectedItem.category }}</span>
          <button
            class="text-slate-400 hover:text-slate-600 font-bold"
            @click="selectedItem = null"
          >
            ✕ 關閉
          </button>
        </div>

        <div class="relative h-52 overflow-hidden rounded-xl bg-slate-100 mb-4">
          <img
            v-if="selectedItem.image"
            :src="selectedItem.image"
            :alt="selectedItem.title"
            class="h-full w-full object-cover"
          />
          <div v-else class="flex h-full items-center justify-center text-slate-400">
            尚無物品照片
          </div>
        </div>

        <h2 class="text-xl font-black mb-2">{{ selectedItem.title }}</h2>
        <p class="text-sm text-slate-600 mb-1">📍 地點：{{ selectedItem.location }}</p>
        <p class="text-sm text-slate-600 mb-6">📅 時間：{{ selectedItem.date }}</p>

        <!-- 放在浮動視窗裡的刪除按鈕 -->
        <button
          class="w-full rounded-xl border border-rose-200 bg-rose-50
          py-3 text-sm font-bold text-rose-600 hover:bg-rose-100 transition"
          @click="deleteItem(selectedItem.id)"
        >
          已找到，確定刪除？
        </button>
      </div>
    </div>

    <!-- 真正的刊登表單彈窗 (原本就有的) -->
    <div
      v-if="showForm"
      class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-5 backdrop-blur-xs"
      @click.self="showForm = false"
    >
      <div class="w-full max-w-lg rounded-2xl bg-white p-6 shadow-xl max-h-[90vh] overflow-y-auto">
        <h2 class="text-xl font-black">刊登失物 / 拾獲物品</h2>
        <p class="mt-1 text-sm text-slate-500">
          填寫詳細資訊，讓失主或拾獲人更容易找到。
        </p>

        <div class="mt-5 space-y-4">
          <!-- 類型選擇 -->
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">類型</label>
            <div class="grid grid-cols-2 gap-3">
              <button
                type="button"
                class="py-2.5 rounded-xl text-sm font-bold border transition"
                :class="formType === 'found' ? 'border-emerald-600 bg-emerald-50 text-emerald-700' : 'border-slate-200 text-slate-600'"
                @click="formType = 'found'"
              >
                🟢 我撿到了 (已拾獲)
              </button>
              <button
                type="button"
                class="py-2.5 rounded-xl text-sm font-bold border transition"
                :class="formType === 'lost' ? 'border-amber-600 bg-amber-50 text-amber-700' : 'border-slate-200 text-slate-600'"
                @click="formType = 'lost'"
              >
                🟠 我弄丟了 (正在尋找)
              </button>
            </div>
          </div>

          <!-- 物品名稱 -->
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">物品名稱 *</label>
            <input
              v-model="formTitle"
              type="text"
              placeholder="例如：黑色筆電包、學生證"
              class="w-full rounded-xl border border-slate-200 px-4 py-2.5 text-sm outline-none focus:border-indigo-600"
            />
          </div>

          <!-- 分類選擇 -->
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">分類</label>
            <select
              v-model="formCategory"
              required
              class="w-full rounded-xl border border-slate-200 bg-white px-4 py-2.5 text-sm outline-none focus:border-indigo-600"
            >
              <option value="" disabled selected>請選擇物品分類</option>
              <option value="電子產品">電子產品</option>
              <option value="文具">文具</option>
              <option value="生活用品">生活用品</option>
              <option value="其他">其他</option>
            </select>
          </div>

          <!-- 發現地點 -->
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">地點 *</label>
            <input
              v-model="formLocation"
              type="text"
              placeholder="例如：圖書館 3 樓、體育館外"
              class="w-full rounded-xl border border-slate-200 px-4 py-2.5 text-sm outline-none focus:border-indigo-600"
            />
          </div>

          <!-- 日期 -->
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">日期</label>
            <input
              v-model="formDate"
              type="datetime-local"
              class="w-full rounded-xl border border-slate-200 px-4 py-2.5 text-sm outline-none focus:border-indigo-600"
            />
          </div>

          <!-- 圖片網址 -->
          <div>
            <label class="block text-xs font-bold text-slate-700 mb-1">圖片網址 (選填)</label>
            <input
              v-model="formImage"
              type="text"
              placeholder="貼上圖片網址（例如 Unsplash 圖片連結）"
              class="w-full rounded-xl border border-slate-200 px-4 py-2.5 text-sm outline-none focus:border-indigo-600"
            />
          </div>
        </div>

        <div class="mt-6 flex gap-3">
          <button
            class="flex-1 rounded-xl bg-indigo-600 py-3
            font-bold text-white hover:bg-indigo-700"
            @click="submitItem"
          >
            確認送出
          </button>

          <button
            class="flex-1 rounded-xl border border-slate-200 py-3
            font-bold text-slate-600 hover:bg-slate-100"
            @click="showForm = false"
          >
            取消
          </button>
        </div>
      </div>
    </div>
  </div>
</template>