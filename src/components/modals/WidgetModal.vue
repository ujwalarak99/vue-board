<template>
	<div class="widget-modal">
		<modal name="widget-modal"
			height="auto">
			<div class="header">
				<h2>Add Widget</h2>
			</div>
			<div class="content">
				<form>
					<div class="form-group">
						<label for="name">Name</label>
						<input type="text" class="form-control" placeholder="Widget1" v-model="widgetParams.name">
						<small class="form-text text-muted">Name will identify a widget uniquely</small>
					</div>
					<div class="form-group">
						<label for="datasource">Datasource</label>
						<select v-model="widgetParams.datasource">
							<option v-for="(datasource, idx) in datasources"
								:value="idx"
								:key="idx">
								{{datasource.name}}
							</option>
						</select>
					</div>
					<div class="form-group">
						<label for="type">Type</label>
						<select v-model="widgetParams.type">
							<option v-for="(type, idx) in types" 
								:value="idx"
								:key="idx">
								{{type}}
							</option>
						</select>
					</div>
					<!-- <line-widget-option v-if="widgetParams.type == Widget.LINE"/>
					<gauge-widget-option v-if="widgetParams.type == Widget.GAUGE"/>
					<slider-widget-option v-if="widgetParams.type == Widget.LINE"/> -->
					<component v-bind:is="widgetParams.type"/>
				</form>
			</div>
			<div class="footer">
				<button type="submit" @click="add">Add</button>
				<button type="close" @click="close">Close</button>
			</div>
		</modal>
	</div>
</template>

<script>
// TODO: 
// 1. JSON editor
// 2. Vee Validate
// 3. Style enhancements
// 4. Scroll if form too big

import {Datasource, Format, Type, DataFetcher} from '../../services/Data.js'
import Events from '../../services/Events.js'
import Widget from '../../services/Widget.js'

export default {
	name: 'WidgetModal',

	props: {
		datasources: {
            type: Array,
            required: true,
            default: () => []
        }
	},

	data () {
		return {
			types: Widget.TYPES,
			widgetParams: {
				name: '',
				datasource: {},
				type: Widget.TYPES[0]
			}
		}
	},

	methods: {
		resetFormFields () {

			this.datasourceParams = {
				name: '',
				uri: '',
				options: JSON.stringify({}),
				format: Format.FORMATS[0],
				type: Type.TYPES[0],
				isDeviceShadow: false
			}
		},

		add () {

			let widget = new Datasource(this.datasourceParams.name,
											this.datasourceParams.uri,
											this.datasourceParams.options,
											this.datasourceParams.format,
											this.datasourceParams.type,
											this.datasourceParams.isDeviceShadow)
			
			console.log(datasource)
			this.$bus.$emit(Events.ADD_WIDGET, widget)
			
			this.$modal.hide('widget-modal')
			this.resetFormFields();
		},

		close () {

			this.$modal.hide('widget-modal')
		}
	}
}
</script>

<style>
	.header {
		padding-left: 1rem;
	}

	.footer {
		padding: 0em 2em 2em 2em;
		display: flex;
		justify-content: flex-end;
	}
	
	.footer button {
		margin: 0em 0em 0em 1em;
	}

	.content {
		padding: 0em 1em 1em 1em;
	}

	.form-control {
		display: block;
	}

	.form-group {
		padding: 5px 0px 5px 0px;
	}

	form label {
		font-weight: 600;
	}
</style>
