<script>
	import { Button, Modal } from 'flowbite-svelte';
  	import { writable } from 'svelte/store';
	import {
		Label,
		Fileupload,
		Textarea,
		Input,
		Checkbox,
	} from 'flowbite-svelte';
	import { supabase } from '../../lib/utils/supabase';
	const items = writable([])
	let sizesModal = true;
	let size;
	let textareaprops = {
		id: 'message',
		name: 'description',
		label: 'Item Description',
		rows: 4,
		placeholder: 'Describe your item ...',
	};

	let fileuploadprops = {
		id: 'user_avatar',
	};

	let fileuploadprops2 = {
		id: 'user_avatar2',
	};
	let placement;
	let open = false;

	const setPlacement = (ev) => {
		placement = ev.target.textContent; // text in the button
		open = !open;
	};
	let showTitle;
	let showBrand
	let showAsking_price
	let showListed
	let showDescription


	const addItem = async (title, brand, asking_price, listed) => {
		const { data: { user } } = await supabase.auth.getUser()
		const { data, error } = await supabase.from('items').insert([{title: showTitle, user_id: user.id, brand: showBrand, asking_price: showAsking_price, description: showDescription},])
    	if(error) {
        	return console.error(error);
    	}
};

</script>

<Button
	size="xl"
	on:click={() => {
		size = 'xl';
		sizesModal = true;
	}}>Add Item</Button
>
<form
	action="?/add"
	method="POST"
	on:submit|preventDefault={addItem}
>
	<Modal
		title="Add Item"
		bind:open={sizesModal}
		{size}
		autoclose
		class="w-full"
	>
		<Label
			for="default-input"
			class="block mb-2">What are you selling?</Label
		>
		<Input
			id="default-input"
			placeholder="Item Name"
			for="title"
			name="title"
			bind:value={showTitle}
		/>
		<Label
			for="default-input"
			class="block mb-2">Brand</Label
		>
		<Input
			id="default-input"
			placeholder="..."
			for="brand"
			name="brand"
			bind:value={showBrand}
		/>
		<Label
			for="default-input"
			class="block mb-2">Asking Price</Label
		>
		<Input
			id="default-input"
			placeholder="$"
			for="asking_price"
			name="asking_price"
			type="number"
			bind:value={showAsking_price}
		/>
		<Textarea
			{...textareaprops}
			for="description"
			bind:value={showDescription}
		/>
		<svelte:fragment slot="footer">
			<Button><input
				type="submit"
				value="Submit"
			/></Button>
			<Button color="alternative">Cancel</Button>
		</svelte:fragment>
		<Fileupload {...fileuploadprops} />
	</Modal>
</form>
