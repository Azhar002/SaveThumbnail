 //1. upload the selected thumbnail to firebase storage
                var name = thumbnail["name"];
                var ext = name.substring(name.lastIndexOf("."), name.length);
                var thumbname = new Date().getTime(); 
                var storageRef = firebase.storage().ref(catname + "/" + thumbname + ext);
                var uploadTask = storageRef.put(thumbnail);
                uploadTask.on("state_changed", 
                    function progress(snapshot){
                        var percentage = (snapshot.bytesTransferred / snapshot.totalBytes) * 100; 
                        $("#upload-progress").html(Math.round(percentage) + "%");
                        $("#upload-progress").attr("style", "width:"+percentage + "%");
                    }, 
                    function error(err){
                    }, 
                    function complete(){
                        var thumbnailUrl = uploadTask.snapshot.downloadURL; 
                        
                        var cat = {
                            "thumbnail": thumbnailUrl, 
                            "desc": desc
                        };
                        database.set(cat, function(err){
                            if(err){
                                $("#result").attr("class", "alert alert-danger");
                                $("#result").html(err.message);
                            }else{
                                $("#result").attr("class", "alert alert-success");
                                $("#result").html("Category added");
                            }
                           
                        }); 
                    }
                
                );
            }
        });
    });
