# tfandroid

ClassifierActivity 结构

java层
Graph
TensorFlowInferenceInterface


# 图片识别流程：
 1. ClassifierActivity.java     -> processImageRGBbytes
 2. TensorFlowImageClassifier.java ->recognizeImage
 3. TensorFlowInferenceInterface.java -> feed
 4. TensorFlowInferenceInterface.java -> run
 5. Session.Runner->run->runHelper
 6. Java_org_tensorflow_Session_run
 7. TF_SessionRun
 8. TF_Run_Helper
 9. DirectSession::Run
 10. ExecutorImpl::RunAsync
 11. ExecutorState(args, this))->RunAsync(std::move(done))
 12. ExecutorState::ScheduleReady
 13. ExecutorState::Process
 
 
# 资源文件
 * multibox_location_priors.txt
    1. DetectorActivity.java -> MB_LOCATION_FILE
    2. TensorFlowMultiBoxDetector.create
    
    
 * imagenet_comp_graph_label_strings.txt
    1. 
    2. 
 
 * multibox_model.pb
    1. MB_MODEL_FILE
 
 * tensorflow_inception_graph.pb
    1. ClassifierActivity.java -> MODEL_FILE
    2. TensorFlowInferenceInterface
    3. TensorFlowInferenceInterface.java -> loadGraph
    4. Graph.java -> importGraphDef 
    5. Java_org_tensorflow_Graph_importGraphDef
    6. TF_GraphImportGraphDef
    7. TF_GraphImportGraphDefWithReturnOutputss
    
 
# 资源文件记载流程
1. TensorFlowImageClassifier.create
    

 
核心基类 OpKernel

