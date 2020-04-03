---
title: matplotlib example viz (snippet)
date: 2020-03-03
tags: ["python"]
---
Python matplotlib example 'viz'


## python viz

Python matplotlib example: viz

```python
    
    def visualize_value(self,values,text=True):
        import matplotlib.pyplot as plt
        import matplotlib.colors
        import matplotlib.cm as cm

        if not text:
            plt.text = lambda *args, **kwargs: None

        norm = matplotlib.colors.Normalize(vmin=min(values), vmax=max(values), clip=True)
        cmap = cm.Blues_r
        values = np.array(values).flatten()
        assert len(values) == self.n_col*self.n_row*2
        iterator = iter(values)

        for i,line in enumerate(self.desc):
            for j,value in enumerate(line):
                startY = self.n_row-i
                startX = j
                act_value = next(iterator) 
                color = cmap(norm(act_value))
                if value == 'W':
                    color = (0,0,0,1)
                xs = np.linspace(startX,startX+1,10) 
                plt.fill_between(xs,startY-1,startY,color=color)
                plt.text(startX+0.4,startY-0.5,'%.3g'%act_value)
        
        for i,line in enumerate(self.desc):
            for j,value in enumerate(line):
                startY = self.n_row-i
                startX = j+self.n_col+1
                act_value = next(iterator) 
                color = cmap(norm(act_value))
                if value == 'W':
                    color = (0,0,0,1)
                xs = np.linspace(startX,startX+1,10) 
                plt.fill_between(xs,startY-1,startY,color=color)
                plt.text(startX+0.4,startY-0.5,'%.3g'%act_value)
        

        plt.xlim(0,self.n_col*2+1)
        plt.ylim(0,self.n_row)


    def visualize_qvalue(self,qvalues,text=True,display_format='%.2g',bounds=None):
        import matplotlib.pyplot as plt
        import matplotlib.colors
        import matplotlib.cm as cm
        if not text:
            plt.text = lambda *args, **kwargs: None
        qvalues = np.array(qvalues).flatten()
        assert len(qvalues) == self.n_col*self.n_row*2*4
        iterator = iter(qvalues)
        if bounds:
            vmin,vmax = bounds
        else:
            vmin,vmax = min(qvalues),max(qvalues)
        norm = matplotlib.colors.Normalize(vmin=vmin, vmax=vmax, clip=True)
        cmap = cm.Blues_r


        for i,line in enumerate(self.desc):
            for j,value in enumerate(line):
                startY = self.n_row-i
                startX = j
                act_values = [next(iterator) for _ in range(4)]
                colors = [cmap(norm(act_value)) for act_value in act_values]

                if value == 'W':
                    colors = [(0,0,0,1)]*4

                xs = np.linspace(0,1,10)
                top_v = np.maximum.reduce([xs,1-xs])
                
                plt.fill_between(startX+xs,startY-1+xs,startY-xs,where=[True]*5+[False]*5,color=colors[0])
                plt.text(startX+0.1,startY-0.5,display_format%act_values[0])

                plt.fill_between(startX+xs,startY-1,startY-top_v,color=colors[1])
                plt.text(startX+0.3,startY-0.8,display_format%act_values[1])

                plt.fill_between(startX+xs,startY-1+xs,startY-xs,where=[False]*5+[True]*5,color=colors[2])
                plt.text(startX+0.7,startY-0.5,display_format%act_values[2])


                plt.fill_between(startX+xs,startY-1+top_v,startY,color=colors[3])
                plt.text(startX+0.3,startY-0.2,display_format%act_values[3])

                location = [(startX+0.1,startY-0.5),(startX+0.3,startY-0.8),(startX+0.7,startY-0.5),(startX+0.3,startY-0.2)][np.argmax(act_values)]
                plt.text(*location,s='*')
        for i,line in enumerate(self.desc):
            for j,value in enumerate(line):
                startY = self.n_row-i
                startX = j + self.n_col + 1
                act_values = [next(iterator) for _ in range(4)]
                colors = [cmap(norm(act_value)) for act_value in act_values]

                if value == 'W':
                    colors = [(0,0,0,1)]*4

                xs = np.linspace(0,1,10)
                top_v = np.maximum.reduce([xs,1-xs])
                
                plt.fill_between(startX+xs,startY-1+xs,startY-xs,where=[True]*5+[False]*5,color=colors[0])
                plt.text(startX+0.1,startY-0.5,display_format%act_values[0])

                plt.fill_between(startX+xs,startY-1,startY-top_v,color=colors[1])
                plt.text(startX+0.3,startY-0.8,display_format%act_values[1])

                plt.fill_between(startX+xs,startY-1+xs,startY-xs,where=[False]*5+[True]*5,color=colors[2])
                plt.text(startX+0.7,startY-0.5,display_format%act_values[2])


                plt.fill_between(startX+xs,startY-1+top_v,startY,color=colors[3])
                plt.text(startX+0.3,startY-0.2,display_format%act_values[3])
                location = [(startX+0.1,startY-0.5),(startX+0.3,startY-0.8),(startX+0.7,startY-0.5),(startX+0.3,startY-0.2)][np.argmax(act_values)]
                plt.text(*location,s='*')
        plt.xlim(0,self.n_col*2+1)
        plt.ylim(0,self.n_row)

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
