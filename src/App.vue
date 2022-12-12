<template>
	<main>
		<div id="app">
			<editor
				v-model="content"
				:api-key="key"
				:init="{
				height: 500,
				menubar: false,
				plugins: [
					'a11ychecker',
					'advlist',
					'advcode',
					'advtable',
					'autolink',
					'checklist',
					'export',
					'lists',
					'link',
					'image',
					'charmap',
					'preview',
					'anchor',
					'searchreplace',
					'visualblocks',
					'powerpaste',
					'fullscreen',
					'formatpainter',
					'insertdatetime',
					'media',
					'table',
					'help',
					'wordcount',
				],
				toolbar:
					'undo redo | formatselect | bold italic backcolor | \
						           				 alignleft aligncenter alignright alignjustify | \
						           				 bullist numlist outdent indent | removeformat | link table| image | checklist | export | Chip-Coin | Chip-Terminology-or-Related-Article',
				contextmenu: false,
				selector: '#tinymce',
				branding: false,
				setup: (editor: any) => {
					editor.ui.registry.addButton('Chip-Coin', {
						text: 'Chip-Coin',
						onAction: () => {
							handleClickChipCoin()
						},
					})
					editor.ui.registry.addButton('Chip-Terminology-or-Related-Article', {
						text: 'Chip-Terminology or Related-Article',
						onAction: () => {
							handleClickChipTerminologyOrRelated()
						},
					})
				}
			}"
			/>
		</div>
	</main>
</template>

<style scoped></style>

<script lang="ts" setup>
import Editor from '@tinymce/tinymce-vue'
import { getTinymce } from '@tinymce/tinymce-vue/lib/cjs/main/ts/TinyMCE'
import { watch, ref } from 'vue'

const key = import.meta.env.VITE_TINYMCE_API_KEY
const content = ref('')

enum EHashKeys {
	'chip-coin' = '$',
	'chip-terminology' = '#',
	'chip-related-article' = '#',
}

const handleClickChipCoin = () => {
	const currentContentSelect = getTinymce().activeEditor.selection.getContent({ format: 'text' })
	if (currentContentSelect.trim() === '') return

	if (currentContentSelect.includes(EHashKeys['chip-coin'])) {
		getTinymce().activeEditor.insertContent(currentContentSelect.replace('$', '').replace('#', '').replaceAll('"', ''))
		return
	}

	getTinymce().activeEditor.insertContent(`${EHashKeys['chip-coin']}${currentContentSelect.replace('#', '')}`)
}

const handleClickChipTerminologyOrRelated = () => {
	const currentContentSelect = getTinymce().activeEditor.selection.getContent({ format: 'text' })

	if (currentContentSelect.trim() === '') return

	if (
		currentContentSelect.includes(EHashKeys['chip-terminology']) ||
		currentContentSelect.includes(EHashKeys['chip-related-article'])
	) {
		getTinymce().activeEditor.insertContent(currentContentSelect.replace('$', '').replace('#', '').replaceAll('"', ''))
		return
	}

	getTinymce().activeEditor.insertContent(`${EHashKeys['chip-terminology']}"${currentContentSelect.replace('$', '')}"`)
}

watch(content, (val) => {
	console.log(val)
})
</script>
