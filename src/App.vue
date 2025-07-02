<template>
  <div id="app" class="min-h-screen bg-gray-100">
    <header class="bg-blue-600 text-white p-4">
      <h1 class="text-2xl font-bold text-center">ソードワールド2.5 魔物データ生成ツール</h1>
    </header>

    <div class="container mx-auto p-4 max-w-full">
      <div class="flex flex-col lg:flex-row justify-center gap-8">
        <!-- 入力フォーム -->
        <div class="bg-white rounded-lg shadow-lg p-6 w-full lg:w-96 xl:w-[480px] min-w-0">
          <h2 class="text-xl font-bold mb-4">魔物データ入力</h2>

          <!-- 保存/読み込み -->
          <div class="mb-4 space-x-2">
            <button
              @click="saveData"
              class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600"
            >
              データ保存
            </button>
            <button
              @click="resetData"
              class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600"
            >
              リセット
            </button>
            <input type="file" @change="importData" accept=".json" class="hidden" ref="fileInput" />
            <button
              @click="$refs.fileInput.click()"
              class="bg-gray-500 text-white px-4 py-2 rounded hover:bg-gray-600"
            >
              読み込み
            </button>
          </div>

          <form @submit.prevent class="space-y-4">
            <!-- 魔物レベル1・名称3 -->
            <div class="grid grid-cols-4 gap-4">
              <div>
                <label class="block text-sm font-medium mb-1">魔物レベル</label>
                <input
                  v-model="monsterData.level"
                  type="number"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
              <div class="col-span-3">
                <label class="block text-sm font-medium mb-1">名称</label>
                <input
                  v-model="monsterData.name"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 知能2・知覚2 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">知能</label>
                <select v-model="monsterData.intelligence" class="w-full border rounded px-3 py-2">
                  <option value=""></option>
                  <option value="なし">なし</option>
                  <option value="動物並み">動物並み</option>
                  <option value="低い">低い</option>
                  <option value="人間並み">人間並み</option>
                  <option value="高い">高い</option>
                  <option value="命令を聞く">命令を聞く</option>
                </select>
              </div>
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">知覚</label>
                <input
                  v-model="monsterData.perception"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 反応2・穢れ2 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">反応</label>
                <select v-model="monsterData.reaction" class="w-full border rounded px-3 py-2">
                  <option value=""></option>
                  <option value="友好的">友好的</option>
                  <option value="中立">中立</option>
                  <option value="敵対的">敵対的</option>
                  <option value="腹具合による">腹具合による</option>
                  <option value="命令による">命令による</option>
                </select>
              </div>
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">穢れ</label>
                <input
                  v-model="monsterData.kegare"
                  type="number"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 言語4 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-4">
                <label class="block text-sm font-medium mb-1">言語</label>
                <input
                  v-model="monsterData.language"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 生息地4 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-4">
                <label class="block text-sm font-medium mb-1">生息地</label>
                <input
                  v-model="monsterData.habitat"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 知名度2・弱点値2 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">知名度</label>
                <input
                  v-model="monsterData.knowledge"
                  type="number"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">弱点値</label>
                <input
                  v-model="monsterData.weaknessValue"
                  type="number"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 弱点4 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-4">
                <label class="block text-sm font-medium mb-1">弱点</label>
                <input
                  v-model="monsterData.weakness"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 先制値2・移動速度2 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">先制値</label>
                <input
                  v-model="monsterData.initiative"
                  type="number"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">移動速度</label>
                <input
                  v-model="monsterData.movement"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 生命抵抗力2・精神抵抗力2 -->
            <div class="grid grid-cols-4 gap-4">
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">生命抵抗力</label>
                <input
                  v-model="monsterData.vitality"
                  type="number"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
              <div class="col-span-2">
                <label class="block text-sm font-medium mb-1">精神抵抗力</label>
                <input
                  v-model="monsterData.mental"
                  type="number"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- 攻撃方法 -->
            <h3 class="text-lg font-semibold">攻撃方法</h3>
            <div
              v-for="(attack, index) in monsterData.attacks"
              :key="index"
              class="border p-4 rounded mb-2"
            >
              <div class="grid grid-cols-4 gap-4 mb-2">
                <div class="col-span-4">
                  <label class="block text-sm font-medium mb-1">部位名</label>
                  <input
                    v-model="attack.part"
                    type="text"
                    class="w-full border rounded px-3 py-2"
                    placeholder=""
                  />
                </div>
              </div>
              <div class="grid grid-cols-4 gap-4 mb-2">
                <div>
                  <label class="block text-sm font-medium mb-1">命中力</label>
                  <input
                    v-model="attack.accuracy"
                    type="number"
                    class="w-full border rounded px-3 py-2"
                    placeholder=""
                  />
                </div>
                <div>
                  <label class="block text-sm font-medium mb-1">打撃点</label>
                  <input
                    v-model="attack.damage"
                    type="text"
                    class="w-full border rounded px-3 py-2"
                    placeholder=""
                  />
                </div>
                <div>
                  <label class="block text-sm font-medium mb-1">回避力</label>
                  <input
                    v-model="attack.dodge"
                    type="number"
                    class="w-full border rounded px-3 py-2"
                    placeholder=""
                  />
                </div>
                <div>
                  <label class="block text-sm font-medium mb-1">防護点</label>
                  <input
                    v-model="attack.protection"
                    type="number"
                    class="w-full border rounded px-3 py-2"
                    placeholder=""
                  />
                </div>
              </div>
              <div class="grid grid-cols-4 gap-4 mb-2">
                <div class="col-span-2">
                  <label class="block text-sm font-medium mb-1">HP</label>
                  <input
                    v-model="attack.hp"
                    type="number"
                    class="w-full border rounded px-3 py-2"
                    placeholder=""
                  />
                </div>
                <div class="col-span-2">
                  <label class="block text-sm font-medium mb-1">MP</label>
                  <input
                    v-model="attack.mp"
                    type="number"
                    class="w-full border rounded px-3 py-2"
                    placeholder=""
                  />
                </div>
              </div>
              <button
                @click="removeAttack(index)"
                class="mt-2 bg-red-500 text-white px-3 py-1 rounded hover:bg-red-600"
              >
                削除
              </button>
            </div>
            <button
              @click="addAttack"
              class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600"
            >
              攻撃方法追加
            </button>

            <!-- 16. 部位数（分割）・17. コア部位 -->
            <div class="grid grid-cols-2 gap-4">
              <div>
                <label class="block text-sm font-medium mb-1">部位数</label>
                <input
                  v-model="monsterData.parts"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
              <div>
                <label class="block text-sm font-medium mb-1">コア部位</label>
                <input
                  v-model="monsterData.core"
                  type="text"
                  class="w-full border rounded px-3 py-2"
                  placeholder=""
                />
              </div>
            </div>

            <!-- プレビュー表示設定 -->
            <div class="border-t pt-4 mt-4">
              <h3 class="text-lg font-semibold mb-2">プレビュー表示設定</h3>
              <div class="space-y-2">
                <div class="flex items-center">
                  <input
                    id="showPartsInfo"
                    v-model="showPartsInfo"
                    type="checkbox"
                    class="mr-2 h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300 rounded"
                  />
                  <label for="showPartsInfo" class="text-sm font-medium">
                    部位数・コア部位セクションを表示する
                  </label>
                </div>
                <div v-if="showPartsInfo" class="flex items-center ml-6">
                  <input
                    id="showCoreInfo"
                    v-model="showCoreInfo"
                    type="checkbox"
                    class="mr-2 h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300 rounded"
                  />
                  <label for="showCoreInfo" class="text-sm font-medium"> コア部位を表示する </label>
                </div>
              </div>
            </div>

            <!-- 18. 特殊能力 -->
            <div>
              <label class="block text-sm font-medium mb-1">特殊能力</label>
              <textarea
                v-model="monsterData.abilities"
                rows="6"
                class="w-full border rounded px-3 py-2"
                placeholder=""
              ></textarea>
            </div>

            <!-- 19. 戦利品 -->
            <div>
              <label class="block text-sm font-medium mb-1">戦利品</label>
              <div v-for="(loot, idx) in monsterData.loot" :key="idx" class="flex gap-2 mb-2">
                <input
                  v-model="loot.dice"
                  type="text"
                  class="border rounded px-2 py-1 w-1/4"
                  placeholder=""
                />
                <span class="self-center">｜</span>
                <input
                  v-model="loot.item"
                  type="text"
                  class="border rounded px-2 py-1 w-3/4"
                  placeholder=""
                />
                <button
                  type="button"
                  @click="removeLoot(idx)"
                  class="bg-red-500 text-white px-2 py-1 rounded hover:bg-red-600"
                >
                  削除
                </button>
              </div>
              <button
                type="button"
                @click="addLoot"
                class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600"
              >
                戦利品追加
              </button>
            </div>

            <!-- 20. 解説 -->
            <div>
              <label class="block text-sm font-medium mb-1">解説</label>
              <textarea
                v-model="monsterData.description"
                rows="4"
                class="w-full border rounded px-3 py-2"
                placeholder=""
              ></textarea>
            </div>
          </form>
        </div>

        <!-- プレビュー -->
        <div class="bg-white rounded-lg shadow-lg p-6 w-full lg:w-auto">
          <div class="flex flex-col sm:flex-row justify-between items-center mb-4 gap-2">
            <h2 class="text-xl font-bold">プレビュー</h2>
            <button
              @click="exportAsImage"
              class="bg-purple-500 text-white px-4 py-2 rounded hover:bg-purple-600 whitespace-nowrap"
            >
              画像として出力
            </button>
          </div>

          <div class="flex justify-center overflow-x-auto">
            <div
              id="monster-card"
              class="monster-card p-4 border-4 border-gray-700 rounded-lg bg-white shadow-xl min-w-[400px] max-w-[480px] w-full"
              style="font-family: 'Noto Sans JP', sans-serif"
            >
              <!-- ヘッダー -->
              <div class="monster-header mb-2">
                <div class="text-center text-white text-base font-bold bg-gray-700 py-1 rounded">
                  {{ monsterData.level }}　{{ monsterData.name }}
                </div>
              </div>

              <!-- 基本データ -->
              <div class="text-xs p-2 border border-gray-400 rounded mb-2 bg-gray-50">
                <div class="grid grid-cols-2 gap-x-2 gap-y-1 mb-1">
                  <div><strong>魔物レベル：</strong>{{ monsterData.level }}</div>
                  <div><strong>名称：</strong>{{ monsterData.name }}</div>
                  <div><strong>知能：</strong>{{ monsterData.intelligence }}</div>
                  <div><strong>知覚：</strong>{{ monsterData.perception }}</div>
                  <div><strong>反応：</strong>{{ monsterData.reaction }}</div>
                  <div><strong>穢れ：</strong>{{ monsterData.kegare }}</div>
                  <div><strong>言語：</strong>{{ monsterData.language }}</div>
                  <div><strong>生息地：</strong>{{ monsterData.habitat }}</div>
                  <div>
                    <strong>知名度／弱点値：</strong>{{ monsterData.knowledge }}／{{
                      monsterData.weaknessValue
                    }}
                  </div>
                  <div><strong>弱点：</strong>{{ monsterData.weakness }}</div>
                  <div><strong>先制値：</strong>{{ monsterData.initiative }}</div>
                  <div><strong>移動速度：</strong>{{ monsterData.movement }}</div>
                  <div>
                    <strong>生命抵抗力：</strong>{{ monsterData.vitality }}({{
                      monsterData.vitality + 7
                    }})
                  </div>
                  <div>
                    <strong>精神抵抗力：</strong>{{ monsterData.mental }}({{
                      monsterData.mental + 7
                    }})
                  </div>
                </div>
              </div>

              <!-- 攻撃データテーブル -->
              <div class="text-xs mb-2">
                <table class="w-full border border-gray-700 text-xs bg-white">
                  <thead>
                    <tr class="bg-gray-200 border-b border-gray-700">
                      <th class="border-r border-gray-400 px-1 py-1">攻撃方法（部位）</th>
                      <th class="border-r border-gray-400 px-1 py-1">命中力</th>
                      <th class="border-r border-gray-400 px-1 py-1">打撃点</th>
                      <th class="border-r border-gray-400 px-1 py-1">回避力</th>
                      <th class="border-r border-gray-400 px-1 py-1">防護点</th>
                      <th class="border-r border-gray-400 px-1 py-1">HP</th>
                      <th class="px-1 py-1">MP</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="attack in displayAttacks"
                      :key="attack.part"
                      class="border-b border-gray-300"
                    >
                      <td class="border-r border-gray-200 px-1 py-1 text-left">
                        {{ attack.part }}
                      </td>
                      <td class="border-r border-gray-200 px-1 py-1 text-center">
                        {{ attack.accuracy }}({{ attack.accuracy + 7 }})
                      </td>
                      <td class="border-r border-gray-200 px-1 py-1 text-center">
                        {{ attack.damage }}
                      </td>
                      <td class="border-r border-gray-200 px-1 py-1 text-center">
                        {{ attack.dodge }}({{ attack.dodge + 7 }})
                      </td>
                      <td class="border-r border-gray-200 px-1 py-1 text-center">
                        {{ attack.protection }}
                      </td>
                      <td class="border-r border-gray-200 px-1 py-1 text-center">
                        {{ attack.hp }}
                      </td>
                      <td class="px-1 py-1 text-center">{{ attack.mp }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>

              <!-- 部位数・コア部位 -->
              <div
                v-if="showPartsInfo"
                class="text-xs p-2 border border-gray-400 rounded mb-2 bg-gray-50"
              >
                <div
                  class="grid gap-x-2 gap-y-1 mb-1"
                  :class="showCoreInfo ? 'grid-cols-2' : 'grid-cols-1'"
                >
                  <div><strong>部位数：</strong>{{ monsterData.parts }}</div>
                  <div v-if="showCoreInfo"><strong>コア部位：</strong>{{ monsterData.core }}</div>
                </div>
              </div>

              <!-- 特殊能力 -->
              <div class="border-gray-700 mt-2 pt-2 mb-2">
                <div class="section-header font-bold mb-1">特殊能力</div>
                <div
                  class="text-xs p-2 bg-gray-50 border border-gray-300 rounded whitespace-pre-line"
                >
                  {{ displayAbilities }}
                </div>
              </div>

              <!-- 戦利品 -->
              <div class="border-gray-700 mt-2 pt-2 mb-2">
                <div class="section-header font-bold mb-1">戦利品</div>
                <div class="text-xs p-2 bg-gray-50 border border-gray-300 rounded">
                  <table class="w-full text-xs border border-gray-400">
                    <tbody>
                      <tr v-for="(loot, idx) in displayLootLines" :key="idx">
                        <td class="border border-gray-400 px-1 py-0.5 text-center w-20">
                          {{ loot.dice }}
                        </td>
                        <td class="border border-gray-400 px-1 py-0.5">{{ loot.item }}</td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>

              <!-- 解説 -->
              <div class="border-gray-700 mt-2 pt-2">
                <div class="section-header font-bold mb-1">解説</div>
                <div
                  class="text-xs p-2 bg-gray-50 border border-gray-300 rounded whitespace-pre-line"
                >
                  {{ displayDescription }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import * as htmlToImage from 'html-to-image'

export default {
  name: 'App',
  data() {
    return {
      monsterData: {
        level: '',
        name: '',
        intelligence: '',
        perception: '',
        reaction: '',
        kegare: '',
        language: '',
        habitat: '',
        knowledge: '',
        weaknessValue: '',
        weakness: '',
        initiative: '',
        movement: '',
        vitality: '',
        mental: '',
        parts: '',
        core: '',
        attacks: [],
        abilities: '',
        loot: [],
        description: '',
      },
      showPartsInfo: true,
      showCoreInfo: true,
    }
  },
  computed: {
    displayAttacks() {
      return this.monsterData.attacks.length > 0 ? this.monsterData.attacks : []
    },
    displayAbilities() {
      return this.monsterData.abilities || ''
    },
    displayLootLines() {
      return Array.isArray(this.monsterData.loot) ? this.monsterData.loot : []
    },
    displayDescription() {
      return this.monsterData.description || ''
    },
  },
  methods: {
    addAttack() {
      this.monsterData.attacks.push({
        part: '',
        accuracy: '',
        damage: '',
        dodge: '',
        protection: 0,
        hp: 0,
        mp: 0,
      })
    },
    removeAttack(index) {
      this.monsterData.attacks.splice(index, 1)
    },
    saveData() {
      const dataStr = JSON.stringify(this.monsterData, null, 2)
      const dataBlob = new Blob([dataStr], { type: 'application/json' })
      const url = URL.createObjectURL(dataBlob)
      const link = document.createElement('a')
      link.href = url
      const now = new Date()
      const pad = (n) => n.toString().padStart(2, '0')
      const y = now.getFullYear()
      const m = pad(now.getMonth() + 1)
      const d = pad(now.getDate())
      const h = pad(now.getHours())
      const min = pad(now.getMinutes())
      const s = pad(now.getSeconds())
      const datetime = `${y}${m}${d}_${h}${min}${s}`
      const level = this.monsterData.level || 'Lv'
      const name = this.monsterData.name || '名称未設定'
      link.download = `${level}-${name}-${datetime}.json`
      link.click()
      URL.revokeObjectURL(url)
    },
    resetData() {
      this.monsterData = {
        level: '',
        name: '',
        intelligence: '',
        perception: '',
        reaction: '',
        kegare: '',
        language: '',
        habitat: '',
        knowledge: '',
        weaknessValue: '',
        weakness: '',
        initiative: '',
        movement: '',
        vitality: '',
        mental: '',
        parts: '',
        core: '',
        attacks: [],
        abilities: '',
        loot: [],
        description: '',
      }
      this.showPartsInfo = true
      this.showCoreInfo = true
    },
    importData(event) {
      const file = event.target.files[0]
      if (file) {
        const reader = new FileReader()
        reader.onload = (e) => {
          try {
            const data = JSON.parse(e.target.result)
            this.monsterData = { ...this.monsterData, ...data }
          } catch (importError) {
            alert('ファイルの読み込みに失敗しました')
          }
        }
        reader.readAsText(file)
      }
    },
    async exportAsImage() {
      const element = document.getElementById('monster-card')
      if (!element) {
        alert('プレビューエリアが見つかりません')
        return
      }

      try {
        // html-to-imageを使用してPNG形式で出力
        const dataUrl = await htmlToImage.toPng(element, {
          quality: 1.0,
          pixelRatio: 2,
          backgroundColor: '#ffffff',
          style: {
            transform: 'scale(1)',
            transformOrigin: 'top left',
            width: element.offsetWidth + 'px',
            height: element.offsetHeight + 'px',
          },
          filter: (node) => {
            // 不要な要素をフィルタリング（必要に応じて）
            return true
          },
          fontEmbedCSS: `
            @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&display=swap');
          `,
          skipFonts: false,
        })

        // 画像をダウンロード
        const link = document.createElement('a')
        const now = new Date()
        const pad = (n) => n.toString().padStart(2, '0')
        const y = now.getFullYear()
        const m = pad(now.getMonth() + 1)
        const d = pad(now.getDate())
        const h = pad(now.getHours())
        const min = pad(now.getMinutes())
        const s = pad(now.getSeconds())
        const datetime = `${y}${m}${d}_${h}${min}${s}`
        const level = this.monsterData.level || 'Lv'
        const name = (this.monsterData.name || '名称未設定').replace(/[<>:"/\\|?*]/g, '_')
        const fileName = `${level}-${name}-${datetime}.png`

        link.download = fileName
        link.href = dataUrl
        document.body.appendChild(link)
        link.click()
        document.body.removeChild(link)
      } catch (err) {
        console.error('画像の生成に失敗しました:', err)
        alert('画像の生成に失敗しました: ' + err.message)
      }
    },
    addLoot() {
      this.monsterData.loot.push({ dice: '', item: '' })
    },
    removeLoot(idx) {
      this.monsterData.loot.splice(idx, 1)
    },
  },
}
</script>
