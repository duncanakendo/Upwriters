<template>
    <div class="container" v-if="$gate.isAdmin()">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card mt-4">
                    <div class="card-header">
                        <h3 class="card-title">Order Details</h3>
                        <div class="card-tools">
                            <a href="/orders">
                                <button type="button" class="btn btn-primary">
                                    <i class="fa fa-hand-point-left"></i>
                                    Back
                                </button>
                            </a>
                        </div>
                    </div>

                    <div class="card-body">
                        <div class="row">
                            <div class="col-md-6">
                                <div class="box">
                                    <div class="box-header">
                                        <h4 class="box-title">Client Details</h4>
                                    </div>
                                    <!-- /.box-header -->
                                    <div class="box-body no-padding table-responsive p-0">
                                        <table class="table">
                                            <tbody>
                                            <tr>
                                                <th>Title</th>
                                                <th style="width: 40px">Details</th>
                                            </tr>
                                            <tr>
                                                <td>Client's Name</td>
                                                <td><span>{{details.name}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Email</td>
                                                <td><span>{{details.email}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Estimated Cost</td>
                                                <td><span>${{details.suggested_price}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Client's Budget</td>
                                                <td><span>${{details.budget}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Agreed Price</td>
                                                <td>
                                                    <span v-if="details.agreedAmount">${{details.agreedAmount}}</span>
                                                    <button @click="agreedModal" v-if="!details.agreedAmount"
                                                            type="button" class="btn btn-sm btn-primary">add
                                                    </button>
                                                </td>
                                            </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <!-- /.box-body -->
                                </div>
                                <!-- /.box -->
                                <hr>
                                <div class="box">
                                    <div class="box-header">
                                        <h4 class="box-title">Order Details</h4>
                                    </div>
                                    <!-- /.box-header -->
                                    <div class="box-body no-padding table-responsive p-0">
                                        <table class="table table-striped">
                                            <tbody>
                                            <tr>
                                                <th>Title</th>
                                                <th style="width: 40px">Details</th>
                                            </tr>
                                            <tr>
                                                <td>Document's Title</td>
                                                <td><span>{{details.title}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Level</td>
                                                <td><span>{{details.level}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Subject</td>
                                                <td>
                                                    <span v-if="details.other_subject">{{details.subject_name}}({{details.other_subject}})</span>
                                                    <span v-if="!details.other_subject">{{details.subject_name}}</span>
                                                </td>
                                            </tr>
                                            <tr>
                                                <td>Document Type</td>
                                                <td><span>{{details.documentType_name}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>No. of Pages</td>
                                                <td><span>{{details.pages}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Deadline</td>
                                                <td><span style="color: red;">{{details.deadline_datetime|myDatetime}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Spacing</td>
                                                <td><span>{{details.spacing}}</span></td>
                                            </tr>
                                            <tr>
                                                <td>Format</td>
                                                <td><span>{{details.format}}</span></td>
                                            </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <!-- /.box-body -->
                                </div>
                                <!-- /.box -->
                            </div>
                            <div class="col-md-6">
                                <div class="box">
                                    <div class="box-header">
                                        <h4 class="box-title">Files</h4>
                                    </div>
                                    <div class="box-body" v-if="this.filesCount > 0">
                                        <div class="row">
                                            <div class="col-md-6 col-sm-6 col-xs-12" v-for="file in files"
                                                 :key="file.id">
                                                <a href="#" @click.prevent="download(file.id, file.path)">
                                                    <div class="info-box">
                                                        <span class="info-box-icon" style="background-color: purple;"><i
                                                                class="fas fa-download"
                                                                style="color: white;"></i></span>

                                                        <div class="info-box-content">
                                                            <span class="info-box-text">{{file.path.substring(18)}}</span>
                                                        </div>
                                                        <!-- /.info-box-content -->
                                                    </div>
                                                </a>
                                                <!-- /.info-box -->
                                            </div>
                                        </div>
                                    </div>
                                    <div class="alert alert-warning alert-dismissible" v-if="this.filesCount == 0">
                                        <button type="button" class="close" data-dismiss="alert" aria-hidden="true">×
                                        </button>
                                        <h5><i class="icon fa fa-ban"></i> Alert!</h5>
                                        No files attached!!
                                    </div>
                                </div>
                                <div class="box">
                                    <div class="box-header">
                                        <h4 class="box-title">Task Description</h4>
                                    </div>
                                    <div class="box-body">
                                        <p>
                                            <a class="btn btn-primary btn-sm" data-toggle="collapse"
                                               href="#collapseExample" role="button" aria-expanded="false"
                                               aria-controls="collapseExample">
                                                <i class="fas fa-eye"></i>
                                                View
                                            </a>
                                        </p>
                                        <div class="collapse" id="collapseExample">
                                            <div class="card card-body">
                                                {{details.task}}
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="box mb-4">
                                    <div class="box-header">
                                        <h5 class="box-title">Upload</h5>
                                    </div>
                                    <div class="box-body">
                                        <button type="button" class="btn btn-success btn-sm" @click="newModal">
                                            <i class="fas fa-cloud-upload-alt"></i>
                                            Upload Completed Task
                                        </button>
                                    </div>
                                </div>
                                <div class="box">
                                    <div class="box-header">
                                        <h5 class="box-title">Completed</h5>
                                    </div>
                                    <div class="box-body" v-if="this.filesCount > 0" style="padding-top: 10px;">
                                        <div class="row">
                                            <div class="col-md-6 col-sm-6 col-xs-12" v-for="complete in completed"
                                                 :key="complete.id">
                                                <a href="#"
                                                   @click.prevent="downloadCompleted(complete.id, complete.path)">
                                                    <div class="info-box">
                                                        <span class="info-box-icon"
                                                              style="background-color: #31d125;"><i
                                                                class="fas fa-download"
                                                                style="color: white;"></i></span>

                                                        <div class="info-box-content">
                                                            <span class="info-box-text">{{complete.path.substring(18)}}</span>
                                                        </div>
                                                        <!-- /.info-box-content -->
                                                    </div>
                                                </a>
                                                <!-- /.info-box -->
                                            </div>
                                        </div>
                                    </div>
                                    <div class="alert alert-warning alert-dismissible" v-if="this.filesCount == 0">
                                        <button type="button" class="close" data-dismiss="alert" aria-hidden="true">×
                                        </button>
                                        <h5><i class="icon fa fa-ban"></i> Alert!</h5>
                                        No files attached!!
                                    </div>
                                    <div class="row">
                                        <div id="paypal-button-container"></div>
                                    </div>
                                </div>
                            </div>

                        </div>
                    </div>
                    <div class="row justify-content-center">
                        <div class="col-md-12">
                            <div class="card-body composer">
                                <textarea v-model="message" placeholder="Write your question here..."></textarea><br>
                                <div class="col-md-10">
                                    <button class="btn btn-success btn-md pull-left" @click="sendMessage"><i
                                            class="fas fa-paper-plane"></i>&nbsp;Send message
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                    <hr>
                    <div class="row">
                        <div class="card-body conversation" @new="handleIncoming">
                            <h1>Messages</h1>
                            <div class=" badge badge-pill badge-primary">{{typing}}</div>
                            <div class="card-body feed" ref="feed">
                                <ul>
                                    <li v-for="message in messages"
                                        :class="`message${message.to == users ? ' sent' : ' received'}`"
                                        :key="message.id">
                                        <div class="text">
                                            <span class="messo">{{ message.text }}</span><br/>
                                            <small class="date">{{message.created_at | myDate}}</small>
                                        </div>
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="modal fade" id="addnew" tabindex="-1" role="dialog" aria-labelledby="addnewLabel"
             aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addnewLabel">Upload File</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="submit()">
                        <div class="modal-body">
                            <div class="form-group">
                                <label for="files">Select File</label>
                                <input type="file" multiple class="form-control-file" @change="fieldChange" id="files">
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                            <button type="submit" class="btn btn-success">
                                <i class="fas fa-cloud-upload-alt"></i>
                                Upload
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
        <div class="modal fade" id="agreed" tabindex="-1" role="dialog" aria-labelledby="addnewLabel"
             aria-hidden="true">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addnewLabel">Agreed Price</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="addPrice()">
                        <div class="modal-body">
                            <div class="form-group">
                                <label for="price">Agreed Price</label>
                                <input v-model="form.price" type="number" class="form-control" name="agreed"
                                       id="price"
                                       placeholder="price" :class="{ 'is-invalid': form.errors.has('price') }">
                                <has-error :form="form" field="price"></has-error>
                            </div>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                            <button type="submit" class="btn btn-success">
                                <i class="fas fa-save"></i>
                                Save
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        props: {
            user: {
                type: Object,
                required: true
            }
        },

        data() {
            return {
                message: '',
                typing: '',
                users: {},
                messages: [],
                orderId: this.$route.params.orderId,
                details: {},
                filesCount: {},
                completed: {},
                files: {},
                attachments: [],
                formf: new FormData(),
                form: new Form({
                    price: '',
                    orderId: this.$route.params.orderId,
                })
            }
        },
        mounted() {
            Echo.private(`message.${this.user.id}`)
                .listen('ChatEvent', (e) => {
                    this.messages.push(e.message);
                })
                .listenForWhisper('typing', (e) => {
                    if (e.name != '') {
                        this.typing = 'typing..'
                    } else {
                        this.typing = ''
                    }
                });
        },
        methods: {
            addPrice() {
                this.form.post('/api/agreed')
                    .then(() => {
                        Fire.$emit('entry');
                        toast.fire({
                            type: 'success',
                            title: 'added successfully'
                        });
                        this.form.reset();
                        $('#agreed').modal('hide');
                    })
                    .catch(error => {
                        this.errors = error.response.data.errors;
                        swal.fire({
                            type: 'error',
                            title: 'Error!!',
                            text: error.response.data.msg,

                        })
                    })
            },
            agreedModal() {
                this.form.reset();
                $('#agreed').modal('show');
            },
            downloadCompleted(id, path) {
                axios.get("/api/downloadcompleted/" + id, {responseType: 'blob'})
                    .then((response) => {
                        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
                        var fileLink = document.createElement('a');
                        console.log(fileLink);
                        fileLink.href = fileURL;
                        fileLink.setAttribute('download', path.substring(18));
                        document.body.appendChild(fileLink);
                        fileLink.click();
                    });
            },
            getCompleted() {
                axios.get("/api/getcompleted/" + this.orderId).then(({data}) => ([this.completed = data]));
            },
            submit() {
                for (let i = 0; i < this.attachments.length; i++) {
                    this.formf.append('files[]', this.attachments[i]);
                }

                const config = {headers: {'Content-Type': 'multipart/form-data'}};

                axios.post('/api/completed/' + this.orderId, this.formf, config).then(response => {
                    Fire.$emit('entry');
                    $('#addnew').modal('hide');
                    this.form.reset();
                    swal.fire({
                        type: 'success',
                        title: 'Submited!!',
                        text: 'Files added successfully',

                    })

                })
                    .catch(response => {
                        //error
                    });
            },
            fieldChange(e) {
                let selectedFiles = e.target.files;
                if (!selectedFiles.length) {
                    return false;
                }
                for (let i = 0; i < selectedFiles.length; i++) {
                    this.attachments.push(selectedFiles[i]);
                }
                console.log(this.attachments);
            },
            newModal() {
                this.form.reset();
                $("#files").val('');
                this.attachments = [];
                $('#addnew').modal('show');
            },
            handleIncoming(message) {
                this.messages.push(message);
            },
            scrollToBottom() {
                setTimeout(() => {
                    this.$refs.feed.scrollTop = this.$refs.feed.scrollHeight - this.$refs.feed.clientHeight;
                }, 50);
            },
            sendMessage() {
                console.log(this.orderId);
                if (this.message == '') {
                    return;
                }
                axios.post('/api/messenger/send', {
                    text: this.message,
                    OrderId: this.orderId,
                    contact_id: this.users,
                }).then((response) => {
                    this.messages.push(response.data);
                    this.message = '';
                })
            },
            download(id, path) {
                axios.get("/api/download/" + id, {responseType: 'blob'})
                    .then((response) => {
                        var fileURL = window.URL.createObjectURL(new Blob([response.data]));
                        var fileLink = document.createElement('a');
                        console.log(fileLink);
                        fileLink.href = fileURL;
                        fileLink.setAttribute('download', path.substring(18));
                        document.body.appendChild(fileLink);
                        fileLink.click();
                    });
            },
            getDetails() {
                axios.get("/api/task/" + this.orderId).then(({data}) => ([this.details = data]));
            },

            getFilesCount() {
                axios.get("/api/ifFiles/" + this.orderId).then(({data}) => ([this.filesCount = data]));
            },

            getFiles() {
                axios.get("/api/getFiles/" + this.orderId).then(({data}) => ([this.files = data]));
            },
            getUser() {
                if (this.$gate.isAdmin()) {
                    axios.get("/api/getUser/" + this.orderId).then(({data}) => ([this.users = data]));
                }
                if (this.$gate.isStudent()) {
                    axios.get("/api/getAdmin/").then(({data}) => ([this.users = data.data]));
                }

            },
            getMessages() {
                axios.get("/api/getMessage/" + this.orderId).then((response) => (this.messages = response.data));
            },
        },
        watch: {
            messages(messages) {
                this.scrollToBottom();
            },
            message() {
                Echo.private(`message.${this.user.id}`)
                    .whisper('typing', {
                        name: this.message
                    });
            },
        },
        created() {
            this.getDetails();
            this.getFilesCount();
            this.getMessages();
            this.getUser();
            this.getFiles();
            this.getCompleted();
            Fire.$on('entry', () => {
                this.getCompleted();
                this.getDetails();
            })
        }
    }
</script>
<style lang="scss" scoped>
    .composer textarea {
        width: 80%;
        margin: 10px;
        border-radius: 3px;
        border: 1px solid lightgray;
        padding: 6px;
    }

    .conversation {
        overflow-y: scroll;
        flex: 5;
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        h1 {
            font-size: 20px;
            padding: 10px;
            margin: 0;
            border-bottom: 1px dashed lightgray;
        }
    }

    .messo {
        font-size: 15px;
        font-weight: 700;
    }

    .date {
        color: #9e9e9e;
        font-weight: 700;
    }

    .feed {
        background: #f0f0f0;
        height: 100%;
        max-height: 470px;
        overflow: scroll;

        ul {
            list-style-type: none;
            padding: 5px;

            li {
                &.message {
                    margin: 10px 0;
                    width: 100%;

                    .text {
                        max-width: 400px;
                        border-radius: 5px;
                        padding: 12px;
                        display: inline-block;
                    }

                    &.received {
                        text-align: right;

                        .text {
                            background: #00e676;
                        }
                    }

                    &.sent {
                        text-align: left;

                        .text {
                            background: #81c4f9;
                        }
                    }
                }
            }
        }
    }
</style>
