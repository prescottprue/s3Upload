# S3 Upload Package

### Go package for simplified file uploading to S3
### Setup
	1. Set environment variables that are listed below (Add them to `~/.bash_profile` or set them in bash)
	1. Run `go build` in package folder
	1. Reference Package in another project.

### Environment Variables
You must set environment variables listed below:
	1. S3_ACCESS_KEY --> S3 Access Key (Amazon Admin Panel/Console)
  1. S3_SECRET_KEY --> S3 Secret Key  (Amazon Admin Panel/Console)

### Functions
	
	//Takes File and Upload location as parameters. File is then uploaded to location on S3.
	UploadImg(file io.Reader, location string)

### Example Handler (Using http)

//Accepts file with the name image and a header called location
func UploadHandler(w http.ResponseWriter, r *http.Request) {
  // FORM HANDLER 
  r.ParseMultipartForm(1000000)
  l := r.Header.Get("location")

  f, _, err := r.FormFile("image")
  if err != nil {
   log.Fatal("Can't Find Image ")
  }

  //UploadImg(file, location)
  _, ul := s3Upload.UploadImg(f.(io.Reader), l)
  //[TODO] Respond with Status, description and Image object with url in it
  res, _ := json.Marshal(Image{ul})
  
  w.Header().Set("Content-Type", "application/json")
  w.Write(res)
}
