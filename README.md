# Lame
A high quality MP3 encoder

Latest version: 3.100 (1.5M)

---

# Source
https://sourceforge.net/projects/lame/files/lame/

---

# Compile Shell
https://github.com/kewlbear/lame-ios-build

---

# How to compile

download the latest version(3.100)

download the shell script `build-lame.sh`

make a new folder `lame`

put the two file into `lame`

unzip `lame-3.100.tar.gz`

modify `lame-3.100` to `lame`

modify `build-lame.sh` line 11, like `SCRATCH = "/Users/apple/Desktop/lame"`

open terminal

input:

`cd /Users/apple/Desktop/lame`

`./build-lame.sh` or you may need `chmod +x build-lame.sh` before it

Done!

---

# Usage

```
- (void)audioTranscode_PCMtoMp3
{
    NSString *cafFilePath = @"PCM path";
    NSString *mp3FilePath = @"MP3 path";
    
    @try {
        int read, write;
        
        FILE *pcm = fopen([cafFilePath cStringUsingEncoding:1], "rb");
        FILE *mp3 = fopen([mp3FilePath cStringUsingEncoding:1], "wb");
        
        const int PCM_SIZE = 8192;
        const int MP3_SIZE = 8192;
        short int pcm_buffer[PCM_SIZE*2];
        unsigned char mp3_buffer[MP3_SIZE];
        
        lame_t lame = lame_init();
        lame_set_in_samplerate(lame, 44100);
        lame_set_VBR(lame, vbr_default);
        lame_init_params(lame);
        
        do {
            read = fread(pcm_buffer, 2*sizeof(short int), PCM_SIZE, pcm);
            if (read == 0)
                write = lame_encode_flush(lame, mp3_buffer, MP3_SIZE);
            else
                write = lame_encode_buffer_interleaved(lame, pcm_buffer, read, mp3_buffer, MP3_SIZE);
            
            fwrite(mp3_buffer, write, 1, mp3);
            
        } while (read != 0);
        
        lame_close(lame);
        fclose(mp3);
        fclose(pcm);
    }
    @catch (NSException *exception) {
        NSLog(@"%@",[exception description]);
    }
    @finally {
        
    }
}
```
