<template>
  <div>
    <div class="q-layout-padding">
      <div>
        <div class="row q-gutter-sm items-center">
          <div class="col-xs-12 col-md-4">
            <q-select v-model="tickStrategy" :options="[
              {label: 'None', value: 'none'},
              {label: 'Leaf', value: 'leaf'},
              {label: 'Leaf Filtered', value: 'leaf-filtered'},
              {label: 'Strict', value: 'strict'}
            ]" label="Tick Strategy" />
          </div>
          <div class="col-xs-12 col-md-4">
            <q-toggle v-model="accordion" label="Accordion mode" />
            <q-toggle v-model="dark" label="On dark background" />
            <q-toggle v-model="selectableNodes" label="Selectable nodes" />
          </div>
          <div class="col-xs-12 col-md-4">
            <q-input v-model="filter" label="Filter" />
          </div>
          <div class="col-6 scroll" style="height: 6em;">
            <span class="text-bold">Ticked</span>:<br>{{ ticked }}
          </div>
          <div class="col-6 scroll" style="height: 6em;">
            <span class="text-bold">Expanded</span>:<br>{{ expanded }}
          </div>
          <div v-if="selectableNodes" class="col-xs-12 col-md-6" style="min-height: 60px">
            <span class="text-bold">Selected</span>:<br>{{ selected }}
          </div>
          <div class="col-xs-12 col-md-6">
            <q-btn @click="getNodeByKey" no-caps label="getNodeByKey test" />
            <q-btn @click="expandAll" no-caps label="Expand all" />
          </div>
        </div>
      </div>

      <div class="q-mt-lg q-pa-lg" :class="{'bg-black': dark}">
        <q-tree
          ref="tree"
          :nodes="nodes"
          node-key="label"
          :selected.sync="selected"
          :tick-strategy="tickStrategy"
          :ticked.sync="ticked"
          :expanded.sync="expanded"
          :dark="dark"
          :accordion="accordion"
          :color="color"
          :filter="filter"
          @lazy-load="onLazyLoad"
        >
          <!--
            <div slot="default-header" slot-scope="prop">
              Default H: {{prop.node.label}}
            </div>
            <div slot="default-body" slot-scope="prop">
              Default body
            </div>
          -->
          <div slot="header-custom" slot-scope="prop" class="row items-center">
            <q-icon :name="prop.node.icon" class="q-tree__icon q-mr-sm" />
            <q-avatar class="q-mr-sm">
              <img src="https://cdn.quasar-framework.org/img/boy-avatar.png">
            </q-avatar>
            <div>
              <div class="row items-center">
                <span>{{ prop.node.label }}</span>
                <q-chip color="red" text-color="white" dense>New</q-chip>
              </div>
              <div>Wooooow. Custom</div>
            </div>
          </div>

          <div slot="body-2-1-2-1" slot-scope="prop">
            Content for: {{ prop.key }}
          </div>
        </q-tree>
      </div>

      <div class="invisible" style="height: 500px">Scroll (on purpose)</div>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    color () {
      return this.dark ? 'red' : 'secondary'
    }
  },
  watch: {
    selectableNodes (v) {
      this.selected = v
        ? this.selected || null
        : undefined
    }
  },
  data () {
    /*
    let children = []
    for (let i = 0; i < 500; i += 1) {
      children.push({
        label: 'Node 1.1.1.1.' + (i + 1)
      })
    }
    */

    return {
      selected: null,
      tickStrategy: 'leaf',
      ticked: ['Node 2.2'],
      expanded: ['Node 2.1.4 - Disabled', 'Node 2.1.3 - freeze exp / tickable'],
      selectableNodes: true,
      dark: false,
      accordion: false,
      filter: '',
      defaultExpandAll: false,
      nodes: [
        {
          label: 'Node 1 - filter',
          icon: 'alarm',
          iconColor: 'red',
          children: [
            {
              label: 'Node 1.1 - accordion test on children',
              avatar: 'https://cdn.quasar-framework.org/img/boy-avatar.png',
              children: [
                {
                  label: 'Node 1.1.1 - tick strategy leaf-filtered',
                  tickStrategy: 'leaf-filtered',
                  children: [
                    {
                      label: 'Node 1.1.1.1 - lots of leafs'/* ,
                      children */
                    }
                  ]
                },
                {
                  label: 'Node 1.1.2 - tick strategy leaf',
                  tickStrategy: 'leaf',
                  children: [
                    {
                      label: 'Node 1.1.2.1'
                    }
                  ]
                },
                {
                  label: 'Node 1.1.3 -- not selectable',
                  selectable: false
                },
                {
                  label: 'Node 1.1.4 - not tickable',
                  tickable: false,
                  children: [
                    {
                      label: 'Node 1.1.4.1'
                    }
                  ]
                }
              ]
            },
            {
              label: 'Node 1.2',
              icon: 'map',
              header: 'custom'
            },
            {
              label: 'Node 1.3 - tap on me!',
              img: 'https://cdn.quasar-framework.org/img/mountains.jpg',
              handler: () => {
                this.$q.notify('Tapped on node 1.3')
              }
            }
          ]
        },
        {
          label: 'Node 2',
          children: [
            {
              label: 'Node 2.1',
              children: [
                {
                  label: 'Node 2.1.1'
                },
                {
                  label: 'Node 2.1.1 BIS - no tick present',
                  noTick: true
                },
                {
                  label: 'Node 2.1.2 - tick strategy strict',
                  tickStrategy: 'strict',
                  children: [
                    {
                      label: 'Node 2.1.2.1 - body slot',
                      body: '2-1-2-1',
                      children: [
                        {
                          label: 'Node q',
                          lazy: true
                        },
                        {
                          label: 'Node a',
                          lazy: true
                        }
                      ]
                    },
                    {
                      label: 'Node 2.1.2.2 - body slot & children',
                      body: '2-1-2-1',
                      children: [
                        {
                          label: 'Node 2.1.2.2.1'
                        },
                        {
                          label: 'Node 2.1.2.2.2'
                        }
                      ]
                    },
                    {
                      label: 'Node 2.1.2.3 - header slot',
                      header: '2-1-2-2'
                    }
                  ]
                },
                {
                  label: 'Node 2.1.x - Disabled',
                  disabled: true,
                  children: [
                    {
                      label: 'Node 2.1.x.1'
                    },
                    {
                      label: 'Node 2.1.x.2'
                    }
                  ]
                },
                {
                  label: 'Node 2.1.3 - freeze exp / tickable',
                  expandable: false,
                  tickable: true,
                  children: [
                    {
                      label: 'Node 2.1.3.1'
                    },
                    {
                      label: 'Node 2.1.3.2'
                    }
                  ]
                },
                {
                  label: 'Node 2.1.4 - Disabled',
                  disabled: true,
                  children: [
                    {
                      label: 'Node 2.1.4.1'
                    },
                    {
                      label: 'Node 2.1.4.2'
                    }
                  ]
                }
              ]
            },
            {
              label: 'Node 2.2'
            },
            {
              label: 'Node 2.3',
              children: [
                {
                  label: 'Node 2.3.1',
                  body: '2-1-2-1'
                },
                {
                  label: 'Node 2.3.2',
                  body: '2-1-2-1'
                }
              ]
            },
            {
              label: 'Node 2.4 - Lazy load',
              lazy: true
            },
            {
              label: 'Node 2.5 - Lazy load empty',
              lazy: true
            }
          ]
        }
      ]
    }
  },
  methods: {
    getNodeByKey () {
      console.log(this.$refs.tree.getNodeByKey('Node 2.1.1'))
    },
    expandAll () {
      this.$refs.tree.expandAll()
    },
    onLazyLoad ({ node, key, done, fail }) {
      // call fail() if any error occurs

      setTimeout(() => {
        if (key.indexOf('Lazy load empty') > -1) {
          done([])
          return
        }

        const label = node.label.replace(' - Lazy load', '')

        done([
          { label: `${label}.1` },
          { label: `${label}.2` },
          { label: `${label}.3`, lazy: true },
          {
            label: `${label}.4`,
            children: [
              { label: `${label}.4.1`, lazy: true },
              { label: `${label}.4.2`, lazy: true }
            ]
          }
        ])
      }, 1000)
    }
  }
}
</script>
