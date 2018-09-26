# HTML2CANVAS_CODEIGNITER_AJAX
//Posting the Canvas.DataUrl() using Ajax and CodeIgniter PHP Library
//Hi all, storing canvas.toDataUrl() in CodeIgniter:

//Jquery, Ajax, download [htm2canvas.min.js](https://html2canvas.hertzen.com/)
//in your script tag
      
      html2canvas($('#targetDiv')[0]).then(function(canvas) {
           var dataUrl = canvas.toDataURL();
           var newDataURL = dataUrl.replace(/^data:image\/png/, "data:application/octet-stream"); //do this to clean the url.
           $("#saveBtn").attr("download", "your_pic_name.png").attr("href", newDataURL); 
           //incase you want to create a download link to save the pic locally.
      $.ajax({
            'type': 'post',
            'url': 'link_to_function_in_controller',
            data: {
                //you can add more data here
                'img':newDataURL
            },
            success: function(data){
                console.log(data);
            },
            error: function(data){
                console.log(data);
            }
        });
        
        });


// in your codeigniter controller
        
        public function link_to_function_in_controller(){
            $img = $this->input->post('img'); //get the image string from ajax post
            $img = substr(explode(";",$img)[1], 7); //this extract the exact image
            $target=time().'_img.png'; //rename the image by time
            $image = file_put_contents($_SERVER['DOCUMENT_ROOT'].'/imagefolder/'.$target, 
           base64_decode($img)); //put the image where your image folder directory is located
            
            $data = array(
                'Image' => $target, //note you can add more data
                'date_inserted' => mdate('%Y-%m-%d %H:%i:%s', now())
            );
            
            $result = $this->model_image->post_image($data); //post_image() here should be your function in codeigniter model layer that handles the database, while the model_image is the model name itself where post_image() draws its properties from
            }
