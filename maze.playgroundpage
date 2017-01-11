
import SpriteKit
import PlaygroundSupport

//View Settigs
let viewWidth = 500
let viewHeight = 500
let Qcolumns = 5
let Qrows = 5
let Rdimension: Int = viewWidth / Qcolumns
let Cdimension: Int = viewHeight / Qrows



//principall class
class gameScene: SKScene {
    var cells:[cella] = []
    var stack:[Int] = []
    let startCell: Int = 0
    var actualCell: Int = 0
    
    override func didMove(to view: SKView) {
        self.size =  CGSize(width: viewWidth, height: viewHeight)
        self.backgroundColor = UIColor.black
        actualCell = startCell
//      cell generation, very slow
        for c in 0...Qrows-1 {
            for r in 0...Qcolumns-1 {
                cells.append(cella(coX: r, coY: c))
            }
        }
        cells[actualCell].visited = true
    }


    override func update(_ currentTime: TimeInterval) {
        cells[actualCell].visited = true
        let nextCell = neighbor(X: cells[actualCell].x , Y: cells[actualCell].y )
        if nextCell != -1{
            removeWall(actual: actualCell, next: nextCell)
            stack.append(actualCell)
            actualCell = nextCell
        }else if stack.count > 0{
            actualCell = stack.popLast()!
        }
        draw()
        cells[actualCell].background.fillColor = UIColor.green
    }
    
    func draw(){
        self.removeAllChildren()
        for cel in cells{
            if cel.visited {
                cel.background.fillColor = UIColor.black
            }
            self.addChild(cel.background)
            if cel.walls[0]{
                self.addChild(cel.top)
            }
            if cel.walls[1]{
                self.addChild(cel.left)
            }
            if cel.walls[2]{
                self.addChild(cel.down)
            }
            if cel.walls[3]{
                self.addChild(cel.right)
            }
        }
    }
    
    struct cella{
        let x: Int
        let y: Int
        
        var walls: [Bool]  = [true, true, true, true]
        
        var top: SKShapeNode = SKShapeNode()
        var left: SKShapeNode = SKShapeNode()
        var down: SKShapeNode = SKShapeNode()
        var right: SKShapeNode = SKShapeNode()
        let background: SKShapeNode
        
        var visited = false
        var modified = true
        init(coX: Int, coY: Int){
            self.x = coX
            self.y = coY
            background = SKShapeNode(rect: CGRect(x: 0, y: 0, width: Rdimension, height: Cdimension))
            background.position = CGPoint(x: self.x * Rdimension , y: self.y * Cdimension)
            background.fillColor = UIColor.orange
            background.lineWidth = 0
            background.zPosition = 0
            
            top = drawLine(wh: "top")
            left = drawLine(wh: "left")
            down = drawLine(wh: "down")
            right = drawLine(wh: "right")
        }
        
        func drawLine(wh: String) -> SKShapeNode{
            var point:[CGPoint] = []
            switch wh {
            case "top":
                point = [CGPoint(x: 0,           y: Cdimension),
                         CGPoint(x: Rdimension,  y: Cdimension)]
            case "left":
                point = [CGPoint(x: Rdimension,  y: 0),
                         CGPoint(x: Rdimension,  y: Cdimension)]
            case "down":
                point = [CGPoint(x: 0,           y: 0),
                         CGPoint(x: Rdimension,  y: 0)]
            case "right":
                point = [CGPoint(x: 0,           y: 0),
                         CGPoint(x: 0,           y: Cdimension)]
            default:
                print("wrong type of line")
                break
            }
            let node = SKShapeNode(points: &point, count: point.count)
            node.position = CGPoint(x: self.x * Rdimension, y: self.y * Cdimension)
            node.lineWidth = 2
            node.strokeColor = UIColor.orange
            node.zPosition = 1
            return node
        }
        

    }
    
     func neighbor(X: Int, Y: Int) -> Int
    {
        
        var neighbours: [Int] = []
        let top  =  index(r: X,     c: Y - 1)
        let left =  index(r: X - 1, c: Y)
        let down =  index(r: X,     c: Y + 1)
        let right = index(r: X + 1, c: Y)
        
        if top != -1 && !cells[top].visited{
            neighbours.append(top)
        }
        if left != -1 && !cells[left].visited{
            neighbours.append(left)
        }
        if down != -1 && !cells[down].visited{
            neighbours.append(down)
        }
        if right != -1 && !cells[right].visited{
            neighbours.append(right)
        }
        if neighbours.count > 0{
            let rnd = Int(arc4random_uniform(UInt32(neighbours.count)))
            return neighbours[rnd]
        }
        return -1
    }
    
    func removeWall(actual: Int, next: Int){
        let Rcheck = cells[actual].x - cells[next].x
        if Rcheck == 1 {
            cells[actual].walls[3] = false
            
            cells[next].walls[1] = false
        }else if Rcheck == -1 {
            cells[actual].walls[1] = false
            cells[next].walls[3] = false
        }
        let Ccheck = cells[actual].y - cells[next].y
        if Ccheck == 1 {
            cells[actual].walls[2] = false
            cells[next].walls[0] = false
        }else if Ccheck == -1 {
            cells[actual].walls[0] = false
            cells[next].walls[2] = false
        }
        
    }
    
     func index(r: Int,c: Int) -> Int { //single dimensions array conversion
        if r < 0 || c < 0 || r > Qcolumns - 1 || c > Qrows - 1{
            return -1
        }
        return r + c * Qcolumns
    }


}

//presenting Scene
let skView = SKView(frame: CGRect(origin: CGPoint.zero, size: CGSize(width: viewWidth, height: viewHeight)))
skView.isMultipleTouchEnabled = true
skView.ignoresSiblingOrder = false

let scene = gameScene()
scene.scaleMode = .aspectFit
scene.backgroundColor = UIColor.red
skView.presentScene(scene)
PlaygroundPage.current.liveView =  skView
