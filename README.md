# android_local_llm
Technique to run llms locally in an android device
1. Install Termux (A terminal emulator and unix like environment for android)
   Either the playstore version (works for most devices without any issue from play protect) or from F-Droid
   Playstore link: https://play.google.com/store/apps/details?id=com.termux
   F-Droid link: https://f-droid.org/packages/com.termux/
2. Update pre-installed packages with the latest version
   ```sh
   pkg upgrade
   ```
3. Install the dependencies
   ```sh
   pkg install git cmake
   ```
4. clone the llama.cpp runtime repo and build it
   ```sh
   git clone https://github.com/ggml-org/llama.cpp
   cd llama.cpp
   cmake -B build
   cmake --build build --config Release -j 8
   ```
   Replace -j 8 with the no of logical CPU or threads. Through -j 8 should be suitable for majority of the modern Android devices. For example, if you
   have a quad core processor use ```cmake --build build --config Release -j 4```
5. Post building, download any llm model in gguf format. You can find a lot of them in huggingface https://huggingface.co/
   For advice on models refer to the section at the end
6. Now the tricky part, once the model is downloaded, you need to place the model in a folder where termux have read access to. You can use the following trick. Try to open android --> data folder in your file manager, it will ask you to open it in system files app, open that app. In android system files app, you will find termux in the harberger menu, click on it to access termux home / data folder files.
   In case, this doesn't work try using the fossify file manager.
   Playstore link: https://play.google.com/store/apps/details?id=org.fossify.filemanager
   F-Droid link: https://f-droid.org/packages/org.fossify.filemanager/
   In older versions of android you can provide storage access to termux and can navigate to the directory where the model is downloaded.
   ```sh
   cd /sdcard/Download/
   ```

7. Assuming you have the model downloaded in the current directory (if not, navigate to the directory where you have the model or you can use it's path as well in the below step).
8. Run the llama runtime as a server with the model.
   ```sh
   ~/llama.cpp/build/bin/llama-server -m model.gguf
   ```
   (Replace model.gguf with the exact model name)
   
9. Open your browser and go to the URL http://localhost:8080
    
    Enter your prompts and let the LLM generate response for you.
    
    Tip: You can enable dark theme in the settings from the llama.cpp localhost page.
10. Use ```ctrl + c``` to quit llama.cpp server in termux.

# About Testing
The same is tested on a midrange android device with 6 GB Ram and another Higher end device with 12 GB Ram, with significant performance in both the cases.

# Storage requirements
Current ~2.86 GiB to 3 GiB storage is required in termux for llama.cpp runtime, build files and compilation dependencies
The storage space for model is separate.

# Model selection guide

llama.cpp runtime primarily supports gguf models.

For initial testing purpose you can use a light weight model like gemma3 270m parameters to verify everything works correctly.
Ideally models upto 3b parameters or higher should work, if your device have sufficient RAM. Though 1b parameters models also provide good experience and they seem to be optimised for this use case.
You can use Quantised models like Q4, Q8 instead of half / full precision like F16 or F32 to reduce RAM usage and offer better performance and overall better efficiency. Quantised models even require less storage space.

Try experimenting with different models and share your observations in the issues.



    
    
    
   
