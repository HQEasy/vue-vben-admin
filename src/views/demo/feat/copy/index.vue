<template>
  <div class="p-4">
    <CollapseContainer class="px-20 bg-white w-full h-32 rounded-md" title="Copy Example">
      <div class="flex justify-center">
        <a-input placeholder="请输入" v-model:value="value" />
        <a-button type="primary" @click="handleCopy">Copy</a-button>
      </div>
    </CollapseContainer>
  </div>
</template>
<script lang="ts">
  import { defineComponent, unref, ref } from 'vue';
  import { CollapseContainer } from '/@/components/Container/index';
  import { useCopyToClipboard } from '/@/hooks/web/useCopyToClipboard';
  import { useMessage } from '/@/hooks/web/useMessage';

  export default defineComponent({
    name: 'Copy',
    components: { CollapseContainer },
    setup() {
      const valueRef = ref('');
      const { createMessage } = useMessage();
      const { clipboardRef, copiedRef } = useCopyToClipboard();

      function handleCopy() {
        const value = unref(valueRef);
        if (!value) {
          createMessage.warning('请输入要拷贝的内容！');
          return;
        }
        clipboardRef.value = value;
        if (unref(copiedRef)) {
          createMessage.warning('copy success！');
        }
      }
      return { handleCopy, value: valueRef };
    },
  });
</script>
