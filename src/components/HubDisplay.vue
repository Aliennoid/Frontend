<script>
import SidebarNavs from "./SidebarNavs.vue";
import ColorThemeBox from "./ColorThemeBox.vue";
import HeaderSearchbox from "./HeaderSearchbox.vue";
import ToggleButton from "./ToggleButton.vue";

export default {
  name: "DashboardComponent",
  components: {
    SidebarNavs,
    ColorThemeBox,
    HeaderSearchbox,
    ToggleButton,
  },
  data() {
    return {
      copy(text) {
        navigator.clipboard.writeText(text);
        setTimeout(function () {
          alert(`Copied Script Hub Code`);
        }, 50);
      },
      hub: {},
      color: localStorage.color,
    };
  },
  async created() {
    const response = await fetch(`${window.$BackendURL}/api/v1/script/hub`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        token: localStorage.token,
      }),
    });
    const { Data: hub } = await response.json();
    this.hub = hub;
  },
};
</script>
<template>
  <div class="p-4 text-gray-300">
    <div class="flex">
      <div class="w-full mt-3 grid-cols-2 grid">
        <div>
          <h2 class="text-2xl font-bold">My Script Hub</h2>
          <p :class="`text-sm text-gray-400`">
            BloxSafe automatically creates a script hub for every user.
            <a href="/docs" :class="`text-${color}`">Learn more</a>
          </p>
        </div>
        <div>
          <button
            @click="copy(hub.Content)"
            :class="`bg-${color} float-right text-white rounded-md px-9 py-3`"
          >
            Copy Code
          </button>
        </div>
      </div>
    </div>
    <textarea
      v-model="hub.Content"
      placeholder="Auto-Generated Lua Code Appears Here"
      disabled
      :class="`scrollbar-thin scrollbar-thumb-${color} scrollbar-track-bray-400 overflow-y-scroll rounded text-gray-400 h-screen text-sm bg-bray-500 border border-bray-300 resize-none w-full mt-3  focus:outline-none px-3 py-3`"
    >
    </textarea>
  </div>
</template>
