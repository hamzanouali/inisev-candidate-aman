<template>
  <div class="layout">
    <div class="layout__sidebar">
      <ul class="menu">
        <li class="menu__item">
          <a class="menu__item__link" href="">inbox({{ emails.length }})</a>
        </li>
        <li class="menu__item">
          <a class="menu__item__link" href="">archive({{ archive.length }})</a>
        </li>
        <li class="menu__item">
          <a class="menu__item__link" href="">logout</a>
        </li>
      </ul>
    </div>
    <div class="layout__content">
      <!-- showiing mailbox -->
      <div class="mailbox">
        <h2 class="mailbox__name">inbox</h2>
        <h1 class="mailbox__selection">
          Selected Emails ({{ countSelectedItems }})
        </h1>

        {{ selectedEmail }}

        <!-- mailbox actions bar -->
        <div class="mailbox__actions">
          <input
            class="mailbox__actions__check input__checkbox"
            type="checkbox"
            @click="selectAll"
          />
          <button
            class="mailbox__actions__button"
            @click="markRead(selectedEmail.id - 1)"
          >
            mark as read (r)
          </button>
          <button class="mailbox__actions__button">archive (a)</button>
        </div>

        <!-- listing emails -->
        <div class="email">
          <div
            class="email__item"
            :class="{ 'email__item--read': checkRead(index) }"
            v-for="(email, index) in emails"
            :key="index"
          >
            <input
              class="email__item__check input__checkbox"
              v-model="selectedEmails"
              :value="email"
              type="checkbox"
            />
            <h6 class="email__item__title" @click="viewEmail(email)">
              {{ email.title }}
            </h6>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showModal" class="layout__modal" @click.self="closeModal">
      <div class="layout__modal__content">
        <button
          class="mailbox__actions__button"
          @click="markRead(selectedEmail.id - 1)"
        >
          mark as read (r)
        </button>
        <button
          class="mailbox__actions__button"
          @click="addToArchive(selectedEmail.id - 1)"
        >
          archive (a)
        </button>
        <div class="mailbox__view">
          <h1>{{ selectedEmail.title }}</h1>
          <p>{{ selectedEmail.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "indexPage",
  data() {
    return {
      emails: [
        {
          id: 1,
          title: "Email Number 1",
          body: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that itver the years, sometimes by accident, sometimes on purpose (injected humour and the like).",
          read: false,
        },
        {
          id: 2,
          title: "Email Number 2",
          body: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that itver the years, sometimes by accident, sometimes on purpose (injected humour and the like).",
          read: false,
        },
        {
          id: 3,
          title: "Email Number 3",
          body: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that itver the years, sometimes by accident, sometimes on purpose (injected humour and the like).",
          read: true,
        },
        {
          id: 4,
          title: "Email Number 4",
          body: "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that itver the years, sometimes by accident, sometimes on purpose (injected humour and the like).",
          read: false,
        },
      ],
      selectedEmails: [],
      selectedEmail: null,
      archive: [],
      showModal: false,
    };
  },
  computed: {
    countSelectedItems() {
      return this.selectedEmails.length;
    },
  },
  methods: {
    selectAll() {
      this.emails.forEach((item) => {
        {
          this.selectedEmails.push(item.id);
        }
      });
    },
    viewEmail(email) {
      this.showModal = true;
      this.selectedEmail = email;
    },
    checkRead(index) {
      return this.emails[index].read;
    },
    markRead(index) {
      alert(index);
      this.emails[index].read = !this.emails[index].read;
    },
    addToArchive(index) {
      this.archive.push(this.emails[index]);
      alert("Email Archived");
    },
    closeModal() {
      this.showModal = false;
      this.selectedEmail = null;
    },
  },
};
</script>

<style lang="scss" scoped>
.layout {
  position: relative;
  display: flex;
  min-height: 100vh;
  &__sidebar {
    width: 320px;
    height: 100%;
    background: #232223;
  }
  &__content {
    background: #ececec;
    width: 100%;
    padding: 2rem;
  }
  &__modal {
    position: absolute;
    top: 0;
    right: 0;
    width: 100%;
    height: 100vh;
    background: rgba(255, 0, 0, 0.315);
    &__content {
      position: absolute;
      top: 0;
      right: 0;
      width: 40%;
      height: 100%;
      background: #ffffff;
      padding: 2rem;

      h1,
      p {
        margin-top: 2rem;
      }
    }
  }
}

.menu {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  list-style: none;
  padding: 0 2rem;
  &__item {
    width: 100%;
    color: #232323;
    background: #ececec;
    padding: 0.5rem 1rem;
    margin-top: 1rem;
    border-radius: 5px;
    cursor: pointer;

    &:last-child {
      margin-top: auto;
      margin-bottom: 2rem;
    }

    &__link {
      text-decoration: none;
    }
  }
}

.mailbox {
  &__name {
    text-transform: capitalize;
  }
  &__actions {
    padding: 1rem;
    &__check {
      width: 1.5rem;
      height: 1.5rem;
    }
    &__button {
      padding: 0.5rem 1rem;
      margin-left: 1rem;
    }
  }
}

.email {
  &__item {
    display: flex;
    align-items: center;
    padding: 1rem;
    margin-bottom: 1rem;
    background: #fff;
    border-radius: 5px;
    &__check {
      margin-right: 1rem;
    }
    &--read {
      opacity: 0.5;
    }
  }
}

.input__checkbox {
  width: 1rem;
  height: 1rem;
}
</style>
