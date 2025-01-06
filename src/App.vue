<template>
  <div class="container">
    <div v-if="showType == 1">
      <button class="add-button" @click="addParticipant">新增</button>
      &nbsp;
      <button class="start-button" @click="startPresentation(2)">开始</button>
    </div>
    <div v-else>
      <button class="start-button" @click="startPresentation(1)">返回</button>
      &nbsp;
      <button class="start-button" @click="getNext" v-show="giftShowList.length != giftList.length">下一个</button>
    </div>

    <ul class="participant-list" v-if="showType == 1">
      <li v-for="(participant, index) in participants" :key="index" class="participant-item">
        <input type="text" class="name-input" v-model="participant.name" />
        <span class="input-label">{{ participant.name }} 的礼物 - {{ participant.index }} 号 </span>
        <button class="delete-button" v-show="participants.length > 1" @click="removeParticipant(index)">删除</button>
      </li>
    </ul>
    <ul class="participant-list" v-else>
      <li v-for="(gift, index) in giftShowList" :key="index" class="participant-item bb-1">
        <span class="gift-label">参与人：{{ gift.name }}</span> &nbsp;
        <span class="gift-value">收到：{{gift.index}} - {{ gift.receiveGift }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
function getList(inputList) {
  // 创建一个深拷贝的列表，以避免修改原始列表
  const list = JSON.parse(JSON.stringify(inputList));

  // 打乱列表顺序
  for (let i = list.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [list[i], list[j]] = [list[j], list[i]]; // 交换元素
  }

  // 分配礼物
  return list.map((person, index) => {
    const nextIndex = (index + 1) % list.length; // 计算下一个索引，实现循环
    console.log(person,'sign')
    return {
      ...person,
      index: list[nextIndex].index,
      receiveGift: list[nextIndex].name + '的礼物' // 分配礼物
    };
  });
}

export default {
  data() {
    return {
      showType: 1,
      participants: [
        { name: 'xx', index: 1 },
      ],
      giftList: [],
      giftShowList: [],
      currentIndex: 0
    };
  },
  methods: {
    addParticipant() {
      this.participants.push({ name: '', index: this.participants.length + 1 });
    },
    removeParticipant(index) {
      this.participants.splice(index, 1);
      this.participants.forEach((val,index) => {
        val.index = index + 1
      })
    },
    startPresentation(val) {
      this.showType = val;
      if (val == 2) {
        this.giftList = getList(this.participants);
        this.giftShowList = [this.giftList[this.currentIndex]]
      } else {
        this.giftShowList = [];
        this.currentIndex = 0
      }
    },
    getNext(idx) {
      this.currentIndex += 1;
      this.giftShowList.push(this.giftList[this.currentIndex])
    }
  },
};
</script>
<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}
.add-button,
.start-button {
  margin-bottom: 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
}

.participant-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.participant-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.name-input,
.gift-input {
  flex: 1;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
  margin: 0 10px;
  width: 90px;
}

.input-label {
  display: inline-block;
  padding-right: 12px;
}

.delete-button {
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  background-color: #f44336;
  color: white;
  cursor: pointer;
}

.gift-label {
  border-right: 1px solid #ddd;
  padding-right: 8px;
}

.bb-1 {
  border-bottom: 1px solid #ddd;
  padding-bottom: 8px;
}
</style>