<script lang="ts">
    import defaultImg from '../images/defaultImgSelect.png';
    import audio from "../audios/sunflower-street-drumloop-85bpm-163900.mp3"
    import WaveSurfer from 'wavesurfer.js';
    import { onMount } from 'svelte';

    let loopImgSrc = defaultImg;
    let waveSurfer: WaveSurfer;
    let audioElement: HTMLDivElement;
    let audioFileName: string;

    function handleImageChange(event: Event) {
        const input = event.target as HTMLInputElement;
        const file = input.files?.[0];
        if (file) {
            loopImgSrc = URL.createObjectURL(file);
        }
    }

    function handleAudioChange(event: Event) {
        const input = event.target as HTMLInputElement;
        const file = input.files?.[0];
        if (file && waveSurfer) {
            const url = URL.createObjectURL(file);
            waveSurfer.load(url);
            audioFileName = file.name;
        }
    }

    onMount(() => {
        const audioLink = localStorage.getItem('uploadedAudio');
        audioFileName = localStorage.getItem('audioFileName') || 'Uploaded Audio';
        if (audioLink) {
            waveSurfer = WaveSurfer.create({
                container: audioElement,
                waveColor: '#4800B6',
                progressColor: 'rgba(72, 0, 182, 0.5)',
                barWidth: 7,
                barHeight: 0.5,
                barGap: 3,
                barRadius: 5,
            });

            waveSurfer.load(audioLink);

        } else {
            console.error("No se encontró el audio en localStorage.");
        }
    });

    function togglePlayPause() {
        waveSurfer.playPause();
    }
</script>



<div class="page-container">
    <div class="form-section">
        <div class="section-info">
            <h1>Upload</h1>
            <div class="gradient-line"></div>
        </div>
        <div class="upload-form-container">
            <form action=""> <!--  action="http://localhost:3000/upload" method="POST" enctype="multipart/form-data -->
                <div class="form-content">
                    <div class="left-section">
                        <div class="input-file">
                            <div class="img-container"><img class="loop-img" src={loopImgSrc} alt=""></div>
                            <label class="img-label" for="img-file">
                                <span>Upload Image</span>
                                <input type="file" class="img-btn" name="file" id="img-file" accept="image/*" on:change={handleImageChange}>
                            </label>
                        </div>
                    </div>
                    <div class="right-section">
                        <div class="input-file">
                            <label class="exclude-label" for="">Audio file</label>
                            <div bind:this={audioElement} class="audio-container" id="audio-container">
                                <source src={audio} type="audio" class="audio">
                            </div>
                            <p>{audioFileName}</p>
                            <label for="audio-file">
                                <span>Change</span>
                                <input type="file" name="file" id="audio-file" accept="audio/*" on:change={handleAudioChange}>
                            </label>
                            <button class="pause-play-button" on:click={togglePlayPause}>play/pause</button>
                        </div>
                        <div class="input">
                            <label for="title">Name</label>
                            <input type="text" name="title" id="title" placeholder="Title" required>
                        </div>
                        <div class="input">
                            <label for="description">Description</label>
                            <input type="text" name="description" id="description" placeholder="Description" required>
                        </div>
                        <div class="input">
                            <label for="options">Daw</label>
                            <select name="options" id="options">
                                <option value="" disabled selected hidden>Insert the Daw where you made the loop</option>
                                <option value="option1">Option 1</option>
                                <option value="option2">Option 2</option>
                                <option value="option3">Option 3</option>
                            </select>
                        </div>
                        <button class="submit-btn" type="submit">Upload</button>
                    </div>
                </div>
            </form>
        </div>
    </div>
</div>

<style>

    .page-container {
        display: flex;
        justify-content: center;
        align-items: center;
        transform: translateX(-8%);
    }

    .page-container h1 {
        display: flex;
        justify-content: center;    
        align-items: center;
        transform: translateX(20%);
    }

    .gradient-line {
        height: 6px;
        width: 135%;
        background: linear-gradient(to right, #B700BD, #4800B6);
        border-radius: 5px;
        margin-bottom: 25px;
    }

    .page-container label {
        font-weight: 600;
    }

    .form-content {
        display: flex;
        justify-content: space-between;
        width: 100%;
    }

    .left-section, .right-section {
        display: flex;
        flex-direction: column;
        width: 48%;
    }

    .input-file {
        margin-bottom: 10px;
    }

    .img-container{
        width: 200px;
        height: 200px;
        border-radius: 15px;
        margin-bottom: 15px;
        overflow: hidden; 
    }

    .loop-img {
        width: 100%;
        height: 100%;
        object-fit: cover;/*container is also useful*/
        border-radius: 15px;
        margin-bottom: 15px;
        background-color: #E9E9E9;
    }

    .input input, .input select {
        width: 165%;
        height: 30px;
        padding-top: 5px;
        padding-left: 10px;
        margin-top: 5px;
        margin-bottom: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        margin-right: 20px;
        background-color: #E9E9E9;
        border: 0.5px solid #E9E9E9;
        font-family: 'Utendo', sans-serif;
        font-weight: 600;
    }

    input::placeholder {
        color: #a0a8a2;
        font-weight: 600;
    }

    .submit-btn{
        width: 170%;
        background-color: #4800B6;
        border-radius: 5px;
        border: 0.5px solid #E9E9E9;
        color: #fff;
        text-align: center;
        font-weight: 600;
        font-size: medium;
        margin-top: 20px;
    }

    .submit-btn:hover {
        background-color: #935ce0;
        cursor: pointer;
    }

    .input-file input[type="file"] {
        display: none;
    }

    .input-file label:not(.exclude-label) {
        width: 150px;
        background-color: #4800B6;
        color: white;
        border: none;
        border-radius: 5px;
        padding: 10px 20px;
        cursor: pointer;
    }

    .input-file label:hover:not(.exclude-label) {
        background-color: #935ce0;
    }

    .audio-container{
        background-color: #E9E9E9;
        border-radius: 5px;
        margin-bottom: 20px;
        width: 170%;
        height: 45%;
    }

    .pause-play-button {
        width: 100px;
        height: 40px;
        background-color: #4800B6;
        border: 0.5px solid #777877;
        color: white;
        border-radius: 5px;
        align-items: center;
        justify-content: center;
        text-align: center;
        transform: translateX(170px);
    }

    .pause-play-button:hover {
        background-color: #935ce0;
        cursor: pointer;
    }

    .img-label {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-bottom: 10px;
    }
</style>