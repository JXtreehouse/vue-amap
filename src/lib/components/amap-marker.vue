<template></template>
<script>
import registerMixin from '../mixins/register-component';
import { lngLatTo, pixelTo } from '../utils/convert-helper';
import { compile, mountedVNode } from '../utils/compile';
export default {
  name: 'el-amap-marker',
  mixins: [registerMixin],
  props: [
    'vid',
    'position',
    'offset',
    'icon',
    'content',
    'topWhenClick',
    'bubble',
    'draggable',
    'raiseOnDrag',
    'cursor',
    'visible',
    'zIndex',
    'angle',
    'autoRotation',
    'animation',
    'shadow',
    'title',
    'clickable',
    'shape',
    'extData',
    'label',
    'events',
    'onceEvents',
    'template',
    'vnode'
  ],
  data() {
    let self = this;
    return {
      propsRedirect: {
        template: 'content',
        vnode: 'content'
      },
      converters: {
        shape(options) {
          return new AMap.MarkerShape(options);
        },
        shadow(options) {
          return new AMap.Icon(options);
        },
        template(tpl) {
          return compile(tpl, self);
        },
        vnode(vnode) {
          const _vNode = typeof vnode === 'function' ? vnode(self) : vnode;
          const vNode = mountedVNode(_vNode);
          return vNode;
        }
      },
      handlers: {
        zIndex(index) {
          this.setzIndex(index);
        },
        visible(flag) {
          flag === false ? this.hide() : this.show();
        }
      }
    };
  },
  methods: {
    initComponent(options) {
      this.$amapComponent = new AMap.Marker(options);
    },
    $$getExtData() {
      return this.$amapComponent.getExtData();
    },
    $$getPosition() {
      return lngLatTo(this.$amapComponent.getPosition());
    },
    $$getOffset() {
      return pixelTo(this.$amapComponent.getOffset());
    }
  }
};
</script>
