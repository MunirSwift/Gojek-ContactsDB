# Gojek-ContactsDB

I am sorry, I have many tried to upload image via NSURL HTTPRequest after reading ruby on rail script rb but failed always getting "Internal Server Error" 403

Here is my code to upload file. (even the code which work on my php script can be uploaded file with data but sorry, when i tried to use same code it does not)

Sample code to upload image with data

let params = ["first_name": "munir", "last_name":"abbas", "email":"m7unity@gmail.com", "phone_number":"00923112824994"]
let data = UIImage(name:"splash").pngImage()

var request = URLRequest(url: URL.init(string: url)!)
let boundary = generateBoundaryString()
request.setValue("multipart/form-data; boundary=\(boundary)", forHTTPHeaderField: "Content-Type")
let body = createBody(url: "https://gojek-contacts-app.herokuapp.com/contacts.json", params:params, data:Data)
request.httpMethod = "POST"
request.httpBody = body
let task = URLSession.shared.dataTask(with: request) { [weak self] (data, response, error) -> Void in
    completion(data!)
}
task.resume()
        
        
//  MARK: - Create Body        
func createBody(url: String, params:[String: AnyObject], data:Data) throws -> URLRequest {

    let boundary = generateBoundaryString()
    var body = Data()

    if params != nil {
        for (key, value) in params! {
            body.append("--\(boundary)\r\n")
            body.append("Content-Disposition: form-data; name=\"\(key)\"\r\n\r\n")
            body.append("\(value)\r\n")
        }
    }

    for path in paths {
    
        let filename = "profile_pic"
        let mimetype = "image/png"

        body.append("--\(boundary)\r\n")
        body.append("Content-Disposition: form-data; name=\"\(filePathKey)\"; filename=\"\(filename)\"\r\n")
        body.append("Content-Type: \(mimetype)\r\n\r\n")
        body.append(data)
        body.append("\r\n")
    }

    body.append("--\(boundary)--\r\n")
    return body
}

/// Create boundary string for multipart/form-data request
///
/// - returns:            The boundary string that consists of "Boundary-" followed by a UUID string.

private func generateBoundaryString() -> String {
    return "Boundary-\(UUID().uuidString)"
}
