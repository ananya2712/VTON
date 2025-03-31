flowchart TD
    subgraph User_Interface
        UI_Input["User Inputs:
        - Person Image
        - Garment Image
        - Mask Color
        - Opacity"]
        UI_Submit["Submit Button"]
        UI_Output["User Outputs:
        - Masked Person Image
        - Standalone Mask
        - Detection Results"]
        
        UI_Input --> UI_Submit
        UI_Submit --> UI_Output
    end
    
    subgraph Pipeline_Initialization
        Init["GarmentMaskingPipeline Class Initialization"]
        LoadModel["Load Models:
        - YOLO
        - SAM (Segment Anything Model)
        - YOLOS-Fashionpedia"]
        LoadMappings["Initialize Mappings:
        - Clothing to Body Parts
        - Body Parts Positions"]
        
        Init --> LoadModel
        Init --> LoadMappings
    end
    
    subgraph Process_Flow
        ProcessImgs["process_images()"]
        Pipeline["GarmentMaskingPipeline.process()"]
        Convert["Convert Images:
        - PIL to NumPy
        - RGBA to RGB"]
        ClassifyClothing["classify_clothing():
        - Analyze garment image
        - Determine clothing type"]
        CreateMask["create_garment_mask():
        1. Classify clothing
        2. Find body parts to mask
        3. Detect person with YOLO
        4. Create segmentation with SAM
        5. Generate mask for specific body parts"]
        OverlayMask["Create Overlay:
        - Apply colored mask
        - Adjust opacity"]
        Results["Return Results:
        - Masked image
        - Binary mask
        - Detection text"]
        
        ProcessImgs --> Pipeline
        Pipeline --> Convert
        Pipeline --> CreateMask
        CreateMask --> ClassifyClothing
        Pipeline --> OverlayMask
        Pipeline --> Results
    end
    
    subgraph Helper_Functions
        Download["download_models()"]
    end
    
    subgraph Model_Components
        YOLO["YOLO:
        Person Detection"]
        SAM["SAM:
        Segmentation"]
        Fashion["YOLOS-Fashionpedia:
        Garment Classification"]
    end
    
    UI_Submit --> ProcessImgs
    Init --> Download
    LoadModel --> YOLO
    LoadModel --> SAM
    LoadModel --> Fashion
    Results --> UI_Output
