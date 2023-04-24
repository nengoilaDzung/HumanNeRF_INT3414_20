1. Chuẩn bị
    Cài đặt Python 3.7-3.9 <br />
    Cài đặt Anaconda (Miniconda) cho Python từ 3.7-3.9 (ở thời điểm báo cáo này được ghi thì Pytorch chỉ hỗ trợ những phiên bản đó) <br />
    Khởi tạo môi trường: <br />
        ```
        conda create --name humannerf python=3.7 
        conda activate humannerf 
        ```
    Cài đặt các packages trong file yêu cầu của source code dự án:	 <br />
        ```
        pip install -r requirements.txt
        ```
    Cài đặt CUDA nếu GPU là GPU của Nvidia <br />
    Cài đặt Pytorch tương ứng với phiên bản CUDA hoặc dùng CPU để Render  <br />
    Tải SMPL Model [tại đây](https://smplify.is.tue.mpg.de/).<br />
    Chuyển các đối tượng Chumpy thành các mảng Numpy của file : basicModel_neutral_lbs_10_207_0_v1.0.0.pkl<br />
    Clone source code này và chạy command trong terminal:<br />
        ```
        python tools/clean_ch.py --input-models path-to-models/*.pkl --output-folder output-folder
        ```
    * Lưu ý: Vì source code SMPLX viết ở Python 2 và vì môi trường là Python 3 nên sẽ phải sửa một vài syntax theo đó.<br />
    Copy: file basicModel_neutral_lbs_10_207_0_v1.0.0.pkl vào địa chỉ third_parties/smpl/models<br />
    Lấy ZJU - Mocap Dataset:<br />
	    Ký vào thỏa thuận bởi bên phát triển và mail cho họ qua [địa chỉ](https://github.com/zju3dv/neuralbody/blob/master/INSTALL.md#zju-mocap-dataset) và để lấy dataset: <br />
<br />

2. Thử nghiệm<br />
    Chuẩn bị dataset :<br />
    Chỉnh sửa địa chỉ thư mục ở phần zju_mocap_path của dataset 387 (ví dụ chính) trong file yaml tại tools/prepare_zju_mocap/387.yaml:<br />
    dataset:<br />
        ```
        zju_mocap_path: /path/to/zju_mocap
        subject: '387'
        sex: 'neutral'
        ```
	Chạy đoạn mã sau :<br />
        ```
        cd tools/prepare_zju_mocap
        python prepare_dataset.py --cfg 387.yaml 
        ```
	Khi đó các frame chụp bởi các camera khác nhau sẽ được tổng hợp lại vào folder output để thuận tiện cho việc xử lý các khung hình.<br />
    Render output:<br />
        Render movement:<br />
            ```
            python run.py --type movement --cfg configs/human_nerf/zju_mocap/387/adventure.yaml
            ```
        Render freeview:<br />
            ```
            python run.py --type freeview --cfg configs/human_nerf/zju_mocap/387/adventure.yaml
            freeview.frame_idx 128
            ```
        Render T-pose:<br />
            ```
            python run.py --type tpose --cfg configs/human_nerf/zju_mocap/387/adventure.yaml<br />
            ```
    