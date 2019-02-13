,,openssl req -new -x509 -days <duration> -extensions v3_ca -keyout ca.key -out ca.crt  
openssl genrsa -des3 -out ca.key 2048 (hdbhav)  
openssl req -new -x509 -days 1826 -key ca.key -out ca.crt  
  
,,openssl genrsa -des3 -out server.key 2048   
openssl genrsa -out server.key 2048  
openssl req -new -out server.csr -key server.key {-sha512}  
,,openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days <duration>  
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 360 {-sha512}  
  
,,openssl genrsa -des3 -out client.key 2048  
openssl genrsa -out client.key 2048  
openssl req -new -out client.csr -key client.key  
,,openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days <duration>  
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAserial ./ca.srl -out client.crt -days 3650 -addtrust clientAuth  
  
openssl x509 -in ca.crt -nameopt multiline -subject -noout  
mosquitto_sub -h localhost -t "#" -v --cafile ca.crt -p 8883 -u "admin" -P "{pass}" --cert client.crt --key client.key  
mosquitto_sub -h localhost -t "#" -v --cafile ca.crt -p 8883 -u "admin" -P "{pass}" (FAIL, no client key)  
mosquitto_sub -h localhost -t "#" -v --cafile ca.crt -p 8883 --cert client.crt --key client.key (FAIL, no user/pass)  
mosquitto_sub -t \$SYS/broker/bytes/\# -v --cafile ca.crt  
  
,,Python  
client1.tls_set(‘c:/python34/steve/MQTT-demos/certs/ca.crt’,tls_version=2)  


CI/CD and test cases.


#Posture
Follow,  
https://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/  (skip opencv_contrib)  
apt install build-essential cmake pkg-config  
apt install libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev  
(Install libjasper-dev, libpng12-0, libpng12-dev from xenial repo.)  
dpkg -i libpng12-0_1.2.54-1ubuntu1_amd64.deb  
dpkg -i libpng12-dev_1.2.54-1ubuntu1_amd64.deb  
apt install python3.5-dev  
apt install libgtk-3-dev  
wget -O opencv.zip https://github.com/opencv/opencv/archive/3.4.0.zip  
unzip opencv.zip  
sudo pip install virtualenv virtualenvwrapper  
vi .bashrc (workon cv)  
source .bashrc  
mkvirtualenv cv -p python3  
pip install numpy  
(cd {Downloads}/opencv-3.4.0/)  
mkdir build  
cd build/  
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local  -D INSTALL_PYTHON_EXAMPLES=ON  -D INSTALL_C_EXAMPLES=OFF  -D   PYTHON_EXECUTABLE=~/.virtualenvs/cv/bin/python -D BUILD_EXAMPLES=ON ..  
make -j4  
sudo make install  
sudo ldconfig  
cd /usr/local/lib/python3.5/site-packages/  
sudo mv cv2.cpython-35m-x86_64-linux-gnu.so cv2.so  
cd ~/.virtualenvs/cv/lib/python3.5/site-packages/  
ln -s /usr/local/lib/python3.5/site-packages/cv2.so cv2.so  
cd {src_dir}  
pip3 install -r requirements.txt  
pip3 install h5py  
python extract_single_letters_from_captchas.py  
python train_model.py  
python solve_captchas_with_model.py  

AWS  
SELECT *, timestamp() as ts, sid, parse_time("yyyy-MM-dd HH:mm:ss", ts_r*1000, "Asia/Singapore") as ts_r FROM ..  
