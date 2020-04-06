torrent from collab to google drive


1. Open google colab (prefer university mail due to unlimited storage),

2. Here we will use GPU hardware accelerator which increases the upload size to 350 GB. So go on runtime and then "CHANGE RUNTIME TYPE" 
     and the change the harware accelerator to GPU.

3. We are using a python library which downloads the torrent file.

4. Now just copy paste the code from the ipynb file or the give code below and make sure to run it after it is pasted then copy the next one.

     >  from google.colab import drive
         drive.mount('/content/drive')


     > !apt install python3-libtorrent


      >   import libtorrent as lt
           import time
          import datetime
ses = lt.session()
ses.listen_on(6881, 6891)
params = {
    'save_path': '/content/drive/My Drive/Torrent/',
    'storage_mode': lt.storage_mode_t(2),
    'paused': False,
    'auto_managed': True,
    'duplicate_is_error': True}
link = "__________________________" # PASTE TORRENT/MAGNET LINK HERE
print(link)

handle = lt.add_magnet_uri(ses, link, params)
ses.start_dht()

begin = time.time()
print(datetime.datetime.now())

print ('Downloading Metadata...')
while (not handle.has_metadata()):
    time.sleep(1)
print ('Got Metadata, Starting Torrent Download...')

print("Starting", handle.name())

while (handle.status().state != lt.torrent_status.seeding):
    s = handle.status()
    state_str = ['queued', 'checking', 'downloading metadata', \
            'downloading', 'finished', 'seeding', 'allocating']
    print ('%.2f%% complete (down: %.1f kb/s up: %.1f kB/s peers: %d) %s ' % \
            (s.progress * 100, s.download_rate / 1000, s.upload_rate / 1000, \
            s.num_peers, state_str[s.state]))
    time.sleep(5)

end = time.time()
print(handle.name(), "COMPLETE")

print("Elapsed Time: ",int((end-begin)//60),"min :", int((end-begin)%60), "sec")

print(datetime.datetime.now())