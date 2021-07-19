# zmm sample per eta-partition

import ROOT, csv

def defTH1(title, name, binning) : 
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer, ieta) : 
    hist = defTH1("%s_%s_%s" %(station, layer, ieta), "%s_%s_%s" %(station, layer, ieta), [30, 0, 30])
    for fn in range(180) : 
        f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        #f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == 1 : 
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == -1 : 
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

with open('zmm_st0_ly1.csv', 'w') as file :
    writer = csv.writer(file)
    writer.writerow(["station", "layer", "ieta", "mean"])
    for eta_id in range(8) : # GE2/1 ieta = 16 
        c = ROOT.TCanvas()
        hist = Hits_Counter(0, 1, eta_id + 1)
        hist.Draw()
        c.SaveAs('nu_st0_ly1_eta%s.png' %(eta_id + 1))
        writer.writerow([0, 1, eta_id + 1, hist.GetMean()])

# zmm sample per chamber

import ROOT, csv

def defTH1(title, name, binning) :
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer) :
    hist = defTH1("%s_%s" %(station, layer), "%s_%s" %(station, layer), [70, 0, 70])
    for fn in range(180) :
        f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        #f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == 1 :
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == -1 :
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                # GE1/1 : 36 chambers, We should count the odd/even chamber separately
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

c = ROOT.TCanvas()
hist = Hits_Counter(0, 1)
hist.Draw()
c.SaveAs('station_0_layer_1.png')

# Nu sample per eta-partition

import ROOT, csv

def defTH1(title, name, binning) : 
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer, ieta) : 
    hist = defTH1("%s_%s_%s" %(station, layer, ieta), "%s_%s_%s" %(station, layer, ieta), [20, 0, 20])
    for fn in range(180) : 
        #f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == 1 : 
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_etaPartition[j] == ieta and tree.gemRecHit_region[j] == -1 : 
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

with open('zmm_st0_ly1.csv', 'w') as file :
    writer = csv.writer(file)
    writer.writerow(["station", "layer", "ieta", "mean"])
    for eta_id in range(8) : # GE2/1 ieta = 16 
        c = ROOT.TCanvas()
        hist = Hits_Counter(0, 1, eta_id + 1)
        hist.Draw()
        c.SaveAs('nu_st0_ly1_eta%s.png' %(eta_id + 1))
        writer.writerow([0, 1, eta_id + 1, hist.GetMean()])

# Nu sample per chamber

import ROOT, csv

def defTH1(title, name, binning) :
    if len(binning) == 3 :
        hist = ROOT.TH1D(name, title, binning[0], binning[1], binning[2])
    return hist

# ME0 : 6 layers, 18 chambers, 8 ieta
# GE1/1 : 2 layers, 36 chambers, 8 ieta / We should count odd/even chamber separately
# GE2/1 : 2 layers, 18 chambers, 16 ieta
# count for each ieta : count for all chambers, and fill one histogram
# for region +1, -1


def Hits_Counter(station, layer) :
    hist = defTH1("%s_%s" %(station, layer), "%s_%s" %(station, layer), [70, 0, 70])
    for fn in range(180) :
        #f = ROOT.TFile("/store/user/jlee/RelValZMM_14/crab_zmm_2026D76PU200/210419_200712/0000/muTuple_%s.root" %(fn + 1)) # zmm sample
        f = ROOT.TFile("/store/user/jlee/RelValNuGun/crab_NuGun_2026D76PU200/210502_100147/0000/muTuple_%s.root" %(fn + 1)) # single Nu sample
        tree = f.Get("muNtupleProducer/MuDPGTree")
        for i_ev in range(tree.GetEntries()) : # Event loop : Fill histogram 36 times for one event (2 regions * 18 chambers)
            tree.GetEntry(i_ev)
            chamber_p = []
            chamber_m = []
            for j, i_chamber in enumerate(tree.gemRecHit_chamber) :
                # region == +1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == 1 :
                    chamber_p.append(i_chamber)
                # region == -1
                if tree.gemRecHit_station[j] == station and tree.gemRecHit_layer[j] == layer and tree.gemRecHit_region[j] == -1 :
                    chamber_m.append(i_chamber)
            for num in range(18) : # 18 chambers
                # GE1/1 : 36 chambers, We should count the odd/even chamber separately
                hist.Fill(chamber_p.count(num + 1))
                hist.Fill(chamber_m.count(num + 1))
    return hist

c = ROOT.TCanvas()
hist = Hits_Counter(0, 1)
hist.Draw()
c.SaveAs('station_0_layer_1.png')
