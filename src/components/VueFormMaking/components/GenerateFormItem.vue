<template>
  <el-form-item
    :label-width="isLabel===false||!widget.options.labelWidthStatus?'0px': widgetLabelWidth + 'px'"
    :label="isLabel===false||widget.type==='divider' || !widget.options.labelWidthStatus?'':widget.name"
    :prop="widget.model"
  >
    <template v-if="preview">
      <template v-if="widget.type === 'color'">
        <div style="width: 32px; height: 20px; margin-top: 6px; border-radius: 3px" :style="{'background-color': dataModel}" />
      </template>
      <template v-else-if="widget.type=='switch'">
        <el-switch
          v-model="dataModel"
          :disabled="true"
        />
      </template>
      <template v-else-if="widget.type === 'editor'">
        <div class="previewEditorDiv" v-html="dataModel" />
      </template>
      <template v-else-if="widget.type=='imgupload'">
        <fm-upload
          v-model="dataModel"
          :disabled="widget.options.disabled"
          :style="{'width': widget.options.width}"
          :width="widget.options.size.width"
          :height="widget.options.size.height"
          :token="widget.options.token"
          :domain="widget.options.domain"
          :multiple="widget.options.multiple"
          :length="widget.options.length"
          :is-qiniu="widget.options.isQiniu"
          :is-delete="widget.options.isDelete"
          :min="widget.options.min"
          :is-edit="widget.options.isEdit"
          :action="widget.options.action"
        />
      </template>
      <template v-else-if="widget.type =='rate'">
        <el-rate
          v-model="dataModel"
          :max="widget.options.max"
          :disabled="true"
          :allow-half="widget.options.allowHalf"
        />
      </template>
      <template v-else-if="widget.type === 'divider'">
        <el-divider
          :direction="widget.options.direction"
          :content-position="widget.options.content_position"
        >
          <span
            :style="{
              'font-size': widget.options.font_size,
              'font-family': widget.options.font_family,
              'font-weight': widget.options.font_weight,
              'color': widget.options.font_color
            }"
          >
            {{ widget.options.defaultValue }}
          </span>
        </el-divider>
      </template>
      <template v-else>
        <div>
          {{ dataModel }}
        </div>
      </template>
    </template>
    <template v-else>
      <template v-if="widget.type === 'input'">
        <el-input
          v-if="widget.options.dataType === 'number' || widget.options.dataType === 'integer' || widget.options.dataType === 'float'"
          v-model.number="dataModel"
          :type="widget.options.dataType"
          :placeholder="widget.options.placeholder"
          :style="{width: widget.options.width}"
          :disabled="widget.options.disabled"
        />
        <el-input
          v-else
          v-model="dataModel"
          :type="widget.options.dataType"
          :disabled="widget.options.disabled"
          :placeholder="widget.options.placeholder"
          :style="{width: widget.options.width}"
        />
      </template>

      <template v-if="widget.type === 'textarea'">
        <el-input
          v-model="dataModel"
          type="textarea"
          :rows="5"
          :disabled="widget.options.disabled"
          :placeholder="widget.options.placeholder"
          :style="{width: widget.options.width}"
        />
      </template>

      <template v-if="widget.type === 'number'">
        <el-input-number
          v-model="dataModel"
          :style="{width: widget.options.width}"
          :step="widget.options.step"
          controls-position="right"
          :disabled="widget.options.disabled"
        />
      </template>

      <template v-if="widget.type === 'radio'">
        <el-radio-group
          v-model="dataModel"
          :style="{width: widget.options.width}"
          :disabled="widget.options.disabled"
        >
          <el-radio
            v-for="(item, index) in (widget.options.remote ? widget.options.remoteOptions : widget.options.options)"
            :key="index"
            :style="{display: widget.options.inline ? 'inline-block' : 'block'}"
            :label="item.value"
          >
            <template v-if="widget.options.remote">{{ item.label }}</template>
            <template v-else>{{ widget.options.showLabel ? item.label : item.value }}</template>
          </el-radio>
        </el-radio-group>
      </template>

      <template v-if="widget.type === 'checkbox'">
        <el-checkbox-group
          v-model="dataModel"
          :style="{width: widget.options.width}"
          :disabled="widget.options.disabled"
        >
          <el-checkbox

            v-for="(item, index) in (widget.options.remote ? widget.options.remoteOptions : widget.options.options)"
            :key="index"
            :style="{display: widget.options.inline ? 'inline-block' : 'block'}"
            :label="item.value"
          >
            <template v-if="widget.options.remote">{{ item.label }}</template>
            <template v-else>{{ widget.options.showLabel ? item.label : item.value }}</template>
          </el-checkbox>
        </el-checkbox-group>
      </template>

      <template v-if="widget.type === 'time'">
        <el-time-picker
          v-model="dataModel"
          :is-range="widget.options.isRange"
          :placeholder="widget.options.placeholder"
          :start-placeholder="widget.options.startPlaceholder"
          :end-placeholder="widget.options.endPlaceholder"
          :readonly="widget.options.readonly"
          :disabled="widget.options.disabled"
          :editable="widget.options.editable"
          :clearable="widget.options.clearable"
          :arrow-control="widget.options.arrowControl"
          :value-format="widget.options.format"
          :style="{width: widget.options.width}"
        />
      </template>

      <template v-if="widget.type=='date'">
        <el-date-picker
          v-model="dataModel"
          :type="widget.options.type"
          :placeholder="widget.options.placeholder"
          :start-placeholder="widget.options.startPlaceholder"
          :end-placeholder="widget.options.endPlaceholder"
          :readonly="widget.options.readonly"
          :disabled="widget.options.disabled"
          :editable="widget.options.editable"
          :clearable="widget.options.clearable"
          :value-format="widget.options.timestamp ? 'timestamp' : widget.options.format"
          :format="widget.options.format"
          :style="{width: widget.options.width}"
        />
      </template>

      <template v-if="widget.type =='rate'">
        <el-rate
          v-model="dataModel"
          :max="widget.options.max"
          :disabled="widget.options.disabled"
          :allow-half="widget.options.allowHalf"
        />
      </template>

      <template v-if="widget.type === 'color'">
        <el-color-picker
          v-model="dataModel"
          :disabled="widget.options.disabled"
          :show-alpha="widget.options.showAlpha"
        />
      </template>

      <template v-if="widget.type === 'select'">
        <el-select
          v-model="dataModel"
          :disabled="widget.options.disabled"
          :multiple="widget.options.multiple"
          :clearable="widget.options.clearable"
          :placeholder="widget.options.placeholder"
          :style="{width: widget.options.width}"
          :filterable="widget.options.filterable"
        >
          <el-option v-for="item in (widget.options.remote ? widget.options.remoteOptions : widget.options.options)" :key="item.value" :value="item.value" :label="widget.options.showLabel || widget.options.remote?item.label:item.value" />
        </el-select>
      </template>

      <template v-if="widget.type=='switch'">
        <el-switch
          v-model="dataModel"
          :disabled="widget.options.disabled"
        />
      </template>

      <template v-if="widget.type=='slider'">
        <el-slider
          v-model="dataModel"
          :min="widget.options.min"
          :max="widget.options.max"
          :disabled="widget.options.disabled"
          :step="widget.options.step"
          :show-input="widget.options.showInput"
          :range="widget.options.range"
          :style="{width: widget.options.width}"
        />
      </template>

      <template v-if="widget.type=='imgupload'">
        <fm-upload
          v-model="dataModel"
          :disabled="widget.options.disabled"
          :style="{'width': widget.options.width}"
          :width="widget.options.size.width"
          :height="widget.options.size.height"
          :token="widget.options.token"
          :domain="widget.options.domain"
          :multiple="widget.options.multiple"
          :length="widget.options.length"
          :is-qiniu="widget.options.isQiniu"
          :is-delete="widget.options.isDelete"
          :min="widget.options.min"
          :is-edit="widget.options.isEdit"
          :action="widget.options.action"
        />
      </template>

      <template v-if="widget.type === 'editor'">
        <vue-editor
          v-model="dataModel"
          :disabled="widget.options.disabled"
          :style="{width: widget.options.width}"
        />
      </template>

      <template v-if="widget.type === 'cascader'">
        <el-cascader
          v-model="dataModel"
          :disabled="widget.options.disabled"
          :clearable="widget.options.clearable"
          :placeholder="widget.options.placeholder"
          :style="{width: widget.options.width}"
          :options="widget.options.remoteOptions"
        />
      </template>

      <template v-if="widget.type === 'text'">
        <span
          :style="{
            'font-size': widget.options.font_size,
            'font-family': widget.options.font_family,
            'font-weight': widget.options.font_weight,
            'color': widget.options.font_color
          }"
        >
          {{ widget.options.defaultValue }}
        </span>
      </template>

      <template v-if="widget.type === 'divider'">
        <el-divider
          :direction="widget.options.direction"
          :content-position="widget.options.content_position"
        >
          <span
            :style="{
              'font-size': widget.options.font_size,
              'font-family': widget.options.font_family,
              'font-weight': widget.options.font_weight,
              'color': widget.options.font_color
            }"
          >
            {{ widget.options.defaultValue }}
          </span>
        </el-divider>
      </template>
    </template>

  </el-form-item>
</template>

<script>
import FmUpload from './Upload'

export default {
  name: 'GenetateFormItem',
  components: {
    FmUpload
  },
  /* eslint-disable */
  props: ['widget', 'models', 'rules', 'remote', 'data', 'disabled', 'preview', 'isLabel'],
  data() {
    return {
      widgetLabelWidth: '',
      dataModel: this.models[this.widget.model],
      tableData: []
    }
  },
  watch: {
    dataModel: {
      deep: true,
      handler(val) {
        this.models[this.widget.model] = val
        this.$emit('update:models', {
          ...this.models,
          [this.widget.model]: val
        })
        this.$emit('input-change', val, this.widget.model)
      }
    },
    models: {
      deep: true,
      handler(val) {
        this.dataModel = val[this.widget.model]
      }
    }
  },
  created() {
    if (this.widget.options.remote && this.remote[this.widget.options.remoteFunc]) {
      this.remote[this.widget.options.remoteFunc]((data) => {
        this.widget.options.remoteOptions = data.map(item => {
          return {
            value: item[this.widget.options.props.value],
            label: item[this.widget.options.props.label],
            children: item[this.widget.options.props.children]
          }
        })
      })
    }
    
    if (this.widget.type === 'imgupload' && this.widget.options.isQiniu) {
      this.remote[this.widget.options.tokenFunc]((data) => {
        this.widget.options.token = data
      })
    }

    if (this.disabled === undefined || this.disabled === null) {
      this.widget.options.disabled = false
    } else {
      this.widget.options.disabled = this.disabled
    }

    // label width
    if (this.widget.options.labelWidthDisabled) {
      this.widgetLabelWidth = this.widget.options.labelWidth
    } else if (this.widget.type==='divider') {
      this.widgetLabelWidth = 0
    } else {
      this.widgetLabelWidth = this.data.config.labelWidth
    }
  },
  methods: {
  }
}
</script>

<style>
  .previewEditorDiv > p {
    margin: 0;
  }
</style>
